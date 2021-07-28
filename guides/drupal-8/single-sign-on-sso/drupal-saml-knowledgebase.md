# Drupal SAML Knowledgebase

## Update IDP Certificate \(Annual\).

From time to time the IDP certificate will expire and need to be re-issued by the Identity and Access Management team.  ~~The certificate will likely be received as a .cer file and it needs to be loaded into a config file in Drupal/SimpleSAMLPHP.~~

~~To extract the certificate string from the certificate file, you can use this command \(assuming Linux or MacOS\)~~

~~sudo openssl x509 -inform der -in pf\_test\_cert.cer -text~~

~~You need to remove all the spaces from the openssl command above so the certificate is a single long string \(approx 2500 chars long\)~~

{% hint style="success" %}
You should request the metadata xml from IAM, this will come as a single file per environment - with the extension .xml.
{% endhint %}

Follow these steps:

1. Login to the boston.gov environment \(.i.e dev, stage, prod etc.\) at xxx.boston.gov/user/login
2. Access the simplesaml config system at xxx.boston.gov/simplesaml
3. \(in the SAML config system\) On the _Welcome_ tab, login using the _**Login as administrator**_ ****link in the top right of the welcome box.  The password is available in the `simplesamlphp/config/config.php` file in the private repository.
4. \(in the SAML config system\) Click on the _Federation_  tab and then at the bottom of the page click on XML to _**SimpleSAMLphp metadata converter.**_  On the page that opens, either paste in the metadata file contents, or select the metadata xml file you have been sent, and click the **Parse** button.
5. In the _Converted metadata_ section of the page, copy the entire contents of the **saml20-idp-remote** converted meta data.
6. Finally, The copied content needs to be added to the `saml20-idp-remote.php` file which is found in `simplesamlphp/metadata/saml20-idp-remote.php` folder in the **boston.gov-d8-private**  repository in GitHub.  The file defines a `$metadata` PHP std\_object with a node for each environment \(see box below\).  Select the correct environment node and paste the metadata as follows:  

```text
$metadata['urn:XXX:sso:boston.gov'] = array (

  ...
  
  'keys' => array (
      0 => array (
        'encryption' => false,
        'signing' => true,
        'type' => 'X509Certificate',
        'X509Certificate' => '####new-cert-string',
      ),
    ),
    
    ...

  );
```

{% hint style="info" %}
There is a separate metadata/certificate for each Ping environment we use.   
Currently there are 3, dev, test and prod.
{% endhint %}

Save the file, commit and deploy the code-base to the relevant Acquia environment to test.

{% hint style="danger" %}
The same certificate is used for **Access-Boston** \(web app on AWS\) and **The Hub** Drupal app on Elastic Beanstalk on AWS.  You will need to update the certs on thos systems as well.
{% endhint %}

