# Patterns Library Architecture

{% hint style="success" %}
AWS CloudFront hosts and exposes both:  
`patterns.boston.gov` and  
`assets.boston.gov`
{% endhint %}

## patterns.boston.gov

### Overview

This sub-domain is a centralized, public CDN serving theme/style and branding elements for City of Boston.

### Architecture

* The main fleet pattern library is located in an  **s3 bucket** named `patterns.boston.gov`
* The root folder of this s3 bucket is referenced by a **CloudFront** Distribution \(`arn:aws:cloudfront::251803681989:distribution/E1PA4V9JL1L6RU`\) which is exposed as a public website at the URL `https://d2hk73kicft6oi.cloudfront.net.`  
* The CoB LAN team have created a **sub-domain** `patterns.boston.gov` on the public DNS servers, and this sub-domain has a CNAME which points to`https://d2hk73kicft6oi.cloudfront.net`. 
* The AWS **Certificate Manager** _\(us-east-1\)_ creates manages a AWS wildcard certificate for `patterns.boston.gov` \(why not use cob main wildcard??\).  This certificate is applied to the CloudFront Distribution to secure HTTPS traffic to the sub-domain endpoint/s.

