---
description: >-
  Icons are created and uploaded to an s3 bucket for storage.  The s3 bucket is
  registered with CloudFront so it can act as a CDN for consumption in web
  applications.
---

# Icon Library Architecture

{% hint style="success" %}
CloudFront exposes both:  
`patterns.boston.gov` and  
`assets.boston.gov`
{% endhint %}

## assets.boston.gov

### Overview

This sub-domain is principally designed to act as a centralized, public CDN serving assets \(images, icons and docs\) from the patterns server.  It is on a separate sub-domain from the patterns server to simplify relocation of these assets to a different resource \(i.e. apart from the css and js elements of the fleet patterns server\) in the future should the need arise.

### Architecture

* The icon folder in the fleet pattern library is located inside the main  **s3 bucket** named `patterns.boston.gov`
* The `/assets` folder of this s3 bucket is referenced by a **CloudFront** Distribution \(`arn:aws:cloudfront::251803681989:distribution/E2NTMFQ3KYUMFB`\) which is exposed as a public website at the URL `https://d33pore7x73r0y.cloudfront.net.`  
* The CoB LAN team have created a **sub-domain** `assets.boston.gov` on the public DNS servers, and this sub-domain has a CNAME which points to`https://d33pore7x73r0y.cloudfront.net`. 
* The AWS **Certificate Manager** _\(us-east-1\)_ has created a wildcard certificate for `*.boston.gov` \(why??\).  This certificate is applied to the CloudFront Distribution to secure https traffic to the sub-domain.
* The CoB LAN team have created a second **sub-domain** `assets_sftp.boston.gov` on the public DNS servers, and this sub-domain has a CNAME which points to `s-27207a4a63144da48.server.transfer.us-east-1.amazonaws.com`
* An SFTP server has been created with a custom hostname of `assets_sftp.boston.gov` and users created with home directories pointing to the `/patterns.boston.gov/assets` .

### Local Development

Designers and other individuals who require FTP access to contribute to assets in the patterns library need to do the following:

1. Ask and AWS admin to create an IAM account in AWS \(if not one already\).
2. Have the user \(create and\) load an SSH key onto their account in AWS.
3. [Add the user](https://console.aws.amazon.com/transfer/home?region=us-east-1#/servers/s-27207a4a63144da48) to the Users list in the SFTP server on AWS.
4. Test a connection to the SFTP server at https://assets\_sftp.boston.gov using filezilla or anothe FTP client which can create SFTP connections.
5. \[optional\] Create a local mount on the designers Mac.

