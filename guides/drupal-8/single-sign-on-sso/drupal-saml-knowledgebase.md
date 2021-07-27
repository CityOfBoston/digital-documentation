# Drupal SAML Knowledgebase

## Update IDP Certificate \(Annual\).

From time to time the IDP certificate will expire and need to be re-issued by the Identity and Access Management team.  The certificate will likely be received as a .cer file and it needs to be loaded into a config file in Drupal/SimpleSAMLPHP.

To extract the certificate string from the certificate file, you can use this command \(assuming Linux or MacOS\)

```text
$ sudo openssl x509 -inform der -in pf_test_cert.cer -text
```

You need to remove all the spaces from the openssl command above so the certificate is a single long string \(approx 2500 chars long\)

The string needs to be added to the `saml20-idp-remote.php` file which is found in `simplesamlphp/metadata/saml20-idp-remote.php` folder in the **boston.gov-d8-private**  repository in GitHub.

The file defines a $metadata PHP std\_object with a node for each environment \(see box below\).  Select the correct environment node and update the `keys` sub-node as follows:  

```text
'keys' => array (
    0 => array (
      'encryption' => false,
      'signing' => true,
      'type' => 'X509Certificate',
      'X509Certificate' => '####new-cert-string',
    ),
  ),
```

{% hint style="info" %}
There is a separate certificate for each Ping environment we use.   
Currently there are 3, dev, test and prod.
{% endhint %}

Save the file, commit and deploy the code-base to the relevant Acquia environment to test.

