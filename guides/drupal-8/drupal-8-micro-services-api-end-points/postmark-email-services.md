---
description: City of Boston use PostMark to relay emails.
---

# PostMark Email Services

Code for this service/endpoint is contained in the module `bos_email`.

## REST Endpoints

{% swagger method="post" path="/rest/email_token/create" baseUrl="" summary="Requests an email token." %}
{% swagger-description %}
Provides an email session token which must be supplied as a field when the form is submitted to the

`rest/email_session`

 endpoints.
{% endswagger-description %}

{% swagger-response status="200: OK" description="Creates and saves a new session token." %}
```javascript
{
    "token_session": int
}
```
{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
The session token is saved against the users session on the server.
{% endhint %}

{% swagger method="post" path="/rest/token/remove" baseUrl="" summary="Deletes an existing token." %}
{% swagger-description %}
Invalidates a previously created email session token.
{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "token_session": "removed"
}

or

{
    "token_session": "not found"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/rest/email_session/{server}" baseUrl="" summary="Send an email via PostMark" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="email["token_session"]" type="String" required="true" %}
The session token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["to_address"]" required="true" type="String" %}
The recipient.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["from_address"]" required="true" type="String" %}
The sender.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["subject"]" required="true" type="String" %}
Subject for email.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["message"]" required="false" type="String" %}
Body for message (contactform)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["useHtml"]" type="Int" %}
Should mail be HTML format? 1 or 0
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["template_id"]" type="String" %}
Use a POSTMARK template
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["cc"]" type="String" %}
CC recipients for email.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["bcc"]" type="String" %}
BCC recipients for email.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email["{string}"]" type="String" %}
Any other fields required by templates.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="authorization" type="String" required="true" %}
token {token}
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    'status': 'success',
    'response': 'Message sent'
}

or 

{
    'status': 'success',
    'response': 'Message queued'
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Catch-all for errors." %}
```javascript
{
    'status' => 'error',
    'response' => '{error message}',
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="The authorization header is wrong" %}
```javascript
{
    'status' => 'error',
    'response' => 'could not authenticate',
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="The token provided was not found in the session. It may have been previously used." %}
```javascript
{
        'status': 'error',
        'response': 'invalid token',
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/rest/email/{server}" baseUrl="" summary="[legacy] Send an email via PostMark (legacy)" %}
{% swagger-description %}
Use email_session for additional security.
{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    'status': 'success',
    'response': 'Message sent'
}

or 

{
    'status': 'success',
    'response': 'Message queued'
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Catch-all for errors" %}
```javascript
{
    'status': 'error',
    'response': '{error message}',
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="The authorization header is wrong" %}
```javascript
{
    'status': 'error',
    'response': 'could not authenticate',
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/emails" baseUrl="" summary="[deprecated 2021] Alias for /rest/email/contactform" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}
