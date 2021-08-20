# SQL Proxy \(DBConnector\)

## Requirement

The City have a number of Data services \(e.g. SQL Servers\) that reside on the city network.  There is no mechanism by which data can be accessed from these services from processes that are not resident on the Cityhall network.  This includes boston.gov and other cloud based services.

## Service Specification

[Specification May 2021](https://docs.google.com/document/d/1jgzdX7X7V1SbOmna0Uo5TXbVPJnUKTV7NNdHKcNj4so/edit?usp=sharing)

## API User Guide

The `DBConnector` micro-service is available at:

| Environment | API Base URL |
| :--- | :--- |
| test | **https://dbconnector.digital-staging.boston.gov** |
| prod | **-** |

### Flow

#### 1. Authenticate \(get or refresh an authToken\)

The first step is to post to the ****[**/v1/auth**](sql-proxy-2021.md#authenticate-user) ****endpoint, providing **username** and **password** credentials.  

* If authenticated, an Authentication Token, and Refresh Token will be returned.
* By default, the Authentication Token \(authToken\) is valid for 180 seconds, and the Refresh Token for 900 seconds.
* When your account was setup, a different lifetime may have been specified for authTokens issued using your credentials.
* The authToken inherits [role-based permissions](sql-proxy-2021.md#user-permissions) that have been assigned to your credentials.

The authToken is then passed as a "bearer token" in the "Authroization" header - and can be used multiple times until it expires.

**Example request header**

```text
HEADER "Authorization: Bearer xxxx-xxxxx-xxxxx"
```

{% hint style="info" %}
When the Authentication Token expires, the Refresh Token can be passed to the **/v1/auth/refresh** endpoint and a new Authentication Token will be returned with a 180 second lifetime. A new Refresh Token will also be generated and returned with a 900 second lifetime.   
_You could also just re-authenticate on the **/v1/auth** endpoint, but using the refresh token is faster and more efficient in the back-end because the user is not re-validated \(using the DBConnector database\) -saving database connection overhead._
{% endhint %}

#### 2. Fetch or verify connToken \(optional\)

If you have not saved or been given a connToken \(which is a uuid representing a connection string\) then the connToken can be obtained from the **/v1/connections/:name** endpoint.

**Note:** You will need to pass the authToken in header.

#### 3. Perform database/remoteAPI operation

Depending on the role attached to the authToken \(see [User Permissions](sql-proxy-2021.md#user-permissions)\), you can use the **/select, /insert, /update, /delete** and **/query** endpoints to interact with data on the remote host defined by a connToken.

**Notes:**   
1. You will need to pass the authToken in the "Authorization" header of your request.   
2. You will need to pass the connToken \(and other parameters\) in the body/payload of your request

Results from all endpoints will be returned in JSON format.

### Terminology

<table>
  <thead>
    <tr>
      <th style="text-align:left">Term</th>
      <th style="text-align:left">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>AuthToken</b>
      </td>
      <td style="text-align:left">
        <p>A token which is generated when a user successfully authenticates against
          the <b>/v1/auth</b> endpoint and starts a session.</p>
        <ul>
          <li>This token is used for all subsequent calls to the endpoint during a session.</li>
          <li>The AuthToken has a lifetime which is typically 180s. After that the AuthToken
            expires and needs to be refreshed.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>RefreshToken</b>
      </td>
      <td style="text-align:left">
        <p>A token which can be used to generate a new AuthToken without re-authenticating.
          A new AuthToken with a new lifetime of 180s can be generated at <b>/v1/auth/refresh. </b>
        </p>
        <ul>
          <li>The RefreshToken is generated at the same time as the AuthToken and has
            a lifetime of 900s.</li>
          <li>After the RefreshToken expires, the only way to generate an AuthToken
            is to authenticate against the <b>/v1/auth</b> endpoint.</li>
          <li>The benefit of the RefreshToken is to ease load on the database server
            as AuthToken regeneration at <b>/v1/auth/refresh</b> endpoint does not require
            a database request,</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>User Account</b>
      </td>
      <td style="text-align:left">A user account is required to authenticate, and is identified by a username
        and password. A user account may be only allow connections from a specified
        IPAddress, and will only be allowed to use certain ConnTokens.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>username/userid</b>
      </td>
      <td style="text-align:left">Each user account has a unique string and numeric identifier. The username
        is the string identifier, it must be unique and may be an email address.
        The userid is a system-generated number which can be used to identify the
        user in some endpoint operations.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ConnectionString</b>
      </td>
      <td style="text-align:left">The <code>DBConnector</code> connects to remote (database) environments
        which are either publicly available, or are housed within the City of Boston
        network. To connect to an environment, a connection<b> </b>string is required.
        Typically the connection string contains the following information about
        the target: Host, Port, Driver, Credentials.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ConnToken / ConnectionToken</b>
      </td>
      <td style="text-align:left">
        <p>Each ConnectionString defined within the <code>DBConnector</code> is issued
          a unique ConnToken when it is saved. Any query requests made via the <code>DBConnector</code>  <b>/v1/query</b> or <b>/v1/select</b> endpoints
          provide the ConnToken (rather than a Connection String).</p>
        <ul>
          <li>No Host or Credentials information needs to be stored in the caller system,
            nor passed across the network by the caller.</li>
          <li>No Host or Credentials are passed across the internet from the caller
            system.</li>
          <li>If Credentials need to be changed, the change is done once in the <code>DBConnector</code> and
            all callers will use the new credentials without having to update their
            ConnTokens.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Session</b>
      </td>
      <td style="text-align:left">A session begins when a user authenticates and receives an AuthToken,
        and ends when the AuthToken expires.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Calling System</b>
      </td>
      <td style="text-align:left">The originating application which calls endpoints in this API.</td>
    </tr>
  </tbody>
</table>

### Authentication

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/auth" %}
{% api-method-summary %}
Authenticate User
{% endapi-method-summary %}

{% api-method-description %}
This endpoint is used to initially authenticate the user, and returns an Authentication Token which must be used in the header of all subsequent endpoint calls.  
- The Auth Token has a default lifetime of 180 seconds \(3 min\) \(can be set per username\),  
- The Refresh Token has an additional validity of 180 seconds \(3 min\).
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="username" type="string" required=true %}
A username \(either a name or an email\) which is registered in the `DBConnector` \(see /v1/users\)
{% endapi-method-parameter %}

{% api-method-parameter name="password" type="string" required=true %}
The password set for the username provided.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns an Authentication Token and a Refresh Token, and also the ID of the user, which can/should be used for other endpoint calls.
{% endapi-method-response-example-description %}

```
{
  "userid": 1,
  "authToken": "xxx-xxxx-xxxx",
  "refreshToken": "xxx-xxxx"
}

** If the user is found to be over-using the endpoint, (flooding) then 
   the following will be returned:
{"error": "No Data"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Errors determined while authenticating will return one of the following:
{% endapi-method-response-example-description %}

```
** No body
{"error": "Missing authentication payload"}
** No username in body
{"error": "Missing username/email field"}
** No password in body
{"error": "Missing password field"}

** Username not found
{"error": "Invalid username or password"}
** Password does not match
{"error": "Invalid username or password"}

** Other un expected errors
{"error": "error message text"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
This will be returned if the user is disabled.
{% endapi-method-response-example-description %}

```
{"error": "User Disabled"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
A 403 may be returned if the request came from an IPAddress not listed for this username, an empty JSON object is returned.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If the username/password does not validate, then an empty JSON object is returned.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
If there is an error generating the token, or any other server-side error a 500 status will be returned with an empty JSON string.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/auth/refresh" %}
{% api-method-summary %}
Refresh Authentication Token
{% endapi-method-summary %}

{% api-method-description %}
Using a valid Refresh Token \(provided from **/v1/auth** endpoint\), this endpoint will refresh a current \(or expired\) Authentication Token.  
Also generates a new Refresh Token.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken.  An expired autToken may be used provided it matches the refreshToken and the refreshToken is not expired.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="refresh\_token" type="string" required=true %}
A valid refreshToken
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Successfully generates a new authToken \(and also a new refreshToken\).
{% endapi-method-response-example-description %}

```
*** Normal response
{
    "userid": 6,
    "authToken": "XXXXXX",
    "refreshToken": "XXXXX"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Other errors that may be returned:
{% endapi-method-response-example-description %}

```
*** General Error.
{"error": <explanation>}

*** If no refreshToken was supplied in the body payload.
{"error": "Missing refresh token"}

*** If the refreshToken is not valid.
{"error": "Invalid refresh token"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
If there are issues with the authToken, these are the expected messages:
{% endapi-method-response-example-description %}

```
*** If no authToken was supplied.
{"error": "Missing Authentication Token"}
*** If the AuthToken supplied has expired.
{"error": "Expired Token"}
*** If the token is unknown or badly formatted.
{"error": "Bad Token"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Usually returned because the callers originating IPAddress does not appear to match the whitelist provided in the user Account.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Common Endpoint Error Messages

The following errors can be raised from calls to the various endpoints.

**400: Bad Request**

400 errors usually indicate some issue with the body or querystring submitted to the endpoint. 

A JSON string is returned with an _error_ node in it.  The error description is a short explanation of the issue encountered.

```text
{
    "error": <description>
}
```

**401: Unauthorised**

Generally, 401 errors are returned when there is an issue with the AuthToken provided in he header.

```text
*** If no authToken was supplied
{"error": "Missing Authentication Token"}

*** If the authToken supplied has expired.
{"error": "Expired Token"}

*** If the authToken is unknown or badly formatted
{"error": "Bad Token"}
```

**403: Forbidden**

When a user attempts to perform a task with insufficient permissions, a 403 error is generated.  The 403 Errors raised by `DBConnector` typically do not provide much information, but errors are logged. Typical 403 errors are:

* Authenticating account or using token from an unregistered IPAddress
* Account has insufficient permissions to perform requested task \(see **Permission** description for each endpoint in this guide and also [User Permissions](sql-proxy-2021.md#user-permissions) section\)

```text
{}
```

**200:OK - Flooding**

If too many requests have been made by a user in a given time period, then the user will be blocked and a 200 response code will be returned with an error message.

```text
*** Response if abuse detected
{
    "error": "No Data"
}
```

### User Management

{% api-method method="get" host="https://dbconnector.digital-staging.boston.gov" path="/v1/users" %}
{% api-method-summary %}
List all Users \(paged\)
{% endapi-method-summary %}

{% api-method-description %}
Returns a list of current users.  
  
**Permission:** ADMIN or OWNER.  
_If the optional `page` & `list` parameters are provided, then a paged output will be returned \(sorted by UserID\)_
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer - A valid connToken
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="page" type="number" required=false %}
The page number to return.  
**Note:** Page numbering starts at zero.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="number" required=false %}
The number of users to return for each page.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a JSON Array with user details.  
**Note:** The password field is obfuscated.  
\(see user permissions for roles\)
{% endapi-method-response-example-description %}

```
[
    {
        "ID": 1,
        "Username": "david.upton@boston.gov",
        "Password": "*****",
        "IPAddresses": "",
        "Enabled": true,
        "Role": 4096,
        "TTL": ""
    },
    {...},
    {...}
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://dbconnector.digital-staging.boston.gov" path="/v1/users/:useridentifier" %}
{% api-method-summary %}
List a single User
{% endapi-method-summary %}

{% api-method-description %}
Returns a single user.  
  
**Permission:**  ADMIN, OWNER or the user specified in `useridentifier`.  
i.e. You can only read your own user record unless you are an ADMIN or OWNER.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="useridentifier" type="integer" required=false %}
The useridentifier may be either of:  
- **userID** \(_numeric_\): unique user number  
- **username** \(_string_\): unique username
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid connToken.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a JSON Array with user details.   
**Note:** The password field is not obfuscated.   
\(see user permissions for roles\)
{% endapi-method-response-example-description %}

```
[
    {
        "ID": 1,
        "Username": "david.upton@boston.gov",
        "Password": "the-password",
        "IPAddresses": "",
        "Enabled": true,
        "Role": 4096,
        "Session": null,
        "TTL": ""
    }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://dbconnector.digital-staging.boston.gov" path="/v1/users/:useridentifier/connections" %}
{% api-method-summary %}
List Connections available to a User
{% endapi-method-summary %}

{% api-method-description %}
Provides a list of connection strings that the user has been granted permission to use.  
  
**Permission:** ADMIN, OWNER or user defined by `useridentifier`.  
i.e. You can only read your own user record unless you are an ADMIN or OWNER.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="useridentifier" type="string" required=true %}
The useridentifier may be either of:  
- **userID** \(_numeric_\): unique user number  
- **username** \(_string_\): unique username
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns an array of connection string records which the requested user is permissioned to use.
{% endapi-method-response-example-description %}

```
[
 {
   "Username":"david.upton@boston.gov",
   "userid":1,
   "connid":1,
   "Token":"xx-xx-xx-xx-xx",
   "ConnectionString":"{\"host\":\"MSSQL_CMDB\", \"port\":\"1433\", \"schema\":\"dbo\", \"database\":\"CMDB\", \"user\":\"myUser\", \"password\":\"myPassword\"}",
   "Description":"Updated Dummy",
   "Enabled":true,
   "Count":0,
   "LastUse":
   "2021-08-10T19:30:20.160Z"
  }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/users" %}
{% api-method-summary %}
Add a new User
{% endapi-method-summary %}

{% api-method-description %}
Adds a new user.  
  
**Permission:** ADMIN or OWNER.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="username" type="string" required=true %}
Any unique string to identify this user.  Recommended to use email addresses for human users \(e.g. "someone@boston.gov"\) or a meaningful name built around the calling service name \(e.g. "cmdb\_nightly\_update"\).   
Maximum 100 chars.
{% endapi-method-parameter %}

{% api-method-parameter name="password" type="string" required=true %}
A complex password. The longer and more complex the better. 
{% endapi-method-parameter %}

{% api-method-parameter name="role" type="number" required=true %}
See User Permissions
{% endapi-method-parameter %}

{% api-method-parameter name="enabled" type="number" required=false %}
1 or 0.  Is this account to be created enabled or disabled. \(0=disabled\).
{% endapi-method-parameter %}

{% api-method-parameter name="ipaddresses" type="string" required=false %}
A comma separated list of IPAddresses the user can make requests from.  If this is left blank, then requests are accepted from all IPAddresses.   
Maximum 150 chars.
{% endapi-method-parameter %}

{% api-method-parameter name="ttl" type="string" required=false %}
The lifetime of authTokens generated for this user.  If this is left blank, then 180s will be used.  Format is "xxxm/s" \(e.g. "90s" for 90 seconds, or "3m" for 3 minutes\)  
**Note:** Shorter key lifetimes provide better security.  
Maxmum 10m or 600s.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
The ID of the newly created user.
{% endapi-method-response-example-description %}

```
{
    "id": 16
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
As well as the common error messages, an error will be thrown if:  
- A duplicate username is attempted to be created.  
- The username is more than 100 chars long.
{% endapi-method-response-example-description %}

```
{
    "error": "Username already exists."
}


{
    "error": "Username is more than 100chars or IPAddress is more than 150 chars or ttl is more than 10 chars."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="patch" host="https://dbconnector.digital-staging.boston.gov" path="/v1/users/:useridentifier" %}
{% api-method-summary %}
Update an existing User
{% endapi-method-summary %}

{% api-method-description %}
Updates the specified user with information provided in the payload.  
  
**Permission:** ADMIN or OWNER 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="useridentifier" type="number" required=true %}
The userid.  
Userid is returned from the **/v1/auth** request.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authroization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
If the record was updated.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If no record for the `userid` is found:
{% endapi-method-response-example-description %}

```
{
    "error": "Not Found"
}

*** Tries to rename username with duplicate
 {
     "error": "Username already exists."
 }

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://dbconnector.digital-staging.boston.gov" path="/v1/user/:userid" %}
{% api-method-summary %}
Delete an existing User
{% endapi-method-summary %}

{% api-method-description %}
Deletes the specified user.  
  
**Permission:** ADMIN or OWNER
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter type="number" name="userid" required=true %}
The userid \(number\).  
Userid is returned from the **/v1/auth** request.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
If the record was found and "deleted" \(actually will just be disabled\)
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If the `userid` was not found.
{% endapi-method-response-example-description %}

```
{
    "error": "Not Found"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Connection Strings

A connection string record contains all the information required for a suitable driver to connect to a remote system.

Each connection string record is defined by a unique UUID \(the connToken\) -and also a unique name.

{% hint style="info" %}
The connToken is used to refer to the remote system in [execution endpoints](sql-proxy-2021.md#execute-commands-on-remote-system) so that connectivity details do not need to be stored in and passed from the calling system.
{% endhint %}

{% hint style="success" %}
The connToken \(UUID\) should never change once the connections string record is created, and therefore can be safely stored in the calling system.
{% endhint %}

{% api-method method="get" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connections" %}
{% api-method-summary %}
List all Connections \(paged\)
{% endapi-method-summary %}

{% api-method-description %}
Returns a list of all active remote system connection strings.  
  
**Permission:** ADMIN OR OWNER
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/:token" %}
{% api-method-summary %}
List single Connection
{% endapi-method-summary %}

{% api-method-description %}
Returns the remote system connection string defined by the specified Connection Token.  
  
**Permission:** ADMIN, OWNER or by a user who has permission to use the connection.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
Bearer: A valid authToken.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If the requested connToken is not found in the database.
{% endapi-method-response-example-description %}

```
{ "error": "connToken not found" }
{ "error": "malformed connToken" }
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/find/:name" %}
{% api-method-summary %}
Find connToken
{% endapi-method-summary %}

{% api-method-description %}
Fetch a connection token using the tokens name.  
  
**Permission:** All authenticated users.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name=":name" type="string" required=true %}
The name of a token
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connection/:token/users" %}
{% api-method-summary %}
List Users who may use a Connection
{% endapi-method-summary %}

{% api-method-description %}
Provides a list of users who have been granted permission to use a connection string.  
  
**Permission:** ADMIN or OWNER
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
Bearer: A valid connToken.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connection" %}
{% api-method-summary %}
Add a new Connection
{% endapi-method-summary %}

{% api-method-description %}
Saves a connection string.  
**Permission:** ADMIN or OWNER.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="connectionString" type="string" required=true %}
The connection string, usually as a JSON string.  
`"{  
  \"host\":\"somewhere.com\",   
  \"username\":\"sa\",  
  \"password\": \"asdfasd\"  
}"`
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
A name by which this connection string can be easily referred to.
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
The purpose of the connection string.  
**Tip:** Include the driver and/or type of connection defined.
{% endapi-method-parameter %}

{% api-method-parameter name="enabled" type="number" required=false %}
1 \(enabled\) or 0 \(disabled\).  
Defaults to 1 \(enabled\) 
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
Returns a message with the newly created connToken.
{% endapi-method-response-example-description %}

```
{
    "connToken": "12342343412341"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Errors related to the parameters passed, or caused by insert action.
{% endapi-method-response-example-description %}

```
{
    error: "Missing connectionString in payload"
}
{
    error: "Missing name in payload"
}

*** If the provided Name already exists.
{
    error: "Cannot insert duplicate key row in object 'dbo.connTokens' with unique index 'XXXX'. The duplicate key value is (xxx)."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connection/:token/user/:userid" %}
{% api-method-summary %}
Grant User Permission to Connection
{% endapi-method-summary %}

{% api-method-description %}
Grants a user permission to use a connection string.  
  
**Permission:**  FULL/SUPER, ADMIN or OWNER.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connToken
{% endapi-method-parameter %}

{% api-method-parameter name="userid" type="integer" required=true %}
A userid \(number\).  
\(The userid is returned from the **/v1/auth** request\)
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: a valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
If the permission is granted.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Errors related to the payload or actual insertion of the permission
{% endapi-method-response-example-description %}

```
*** Required fields missing
{
    error: "Missing connectionString in payload"
}
{
    error: "Missing name in payload"
}

*** Token with that Name already exists
{
    error: "Cannot insert duplicate key row in object 'dbo.connTokens' with unique index 'UK_Name'. The duplicate key value is (XXX)."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="patch" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/:token" %}
{% api-method-summary %}
Update an existing Connection
{% endapi-method-summary %}

{% api-method-description %}
Updates the remote system connection string defined by the specified Connection Token.   
  
**Permission**: ADMIN or OWNER..
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connToken.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="connectionString" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="enabled" type="integer" required=false %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
If the connection is successfully updated.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/:token" %}
{% api-method-summary %}
Delete an existing Connection
{% endapi-method-summary %}

{% api-method-description %}
Deletes the remote system connection string defined by the specified Connection Token.  
  
**Permission:** ADMIN or OWNER
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connToken
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
The connection string is disabled.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If the connToken provided cannot be found.
{% endapi-method-response-example-description %}

```
*** Connection string was not found in system.
{
    "error": "Not Found"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://dbconnector.digital-staging.boston.gov" path="/v1/connection/:token/user/:userid" %}
{% api-method-summary %}
Revoke User Permission for Connection
{% endapi-method-summary %}

{% api-method-description %}
Revoke an existing permission for a user to use a connection string.  
  
**Permission:** ADMIN or OWNER
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connToken.
{% endapi-method-parameter %}

{% api-method-parameter type="integer" name="userid" required=true %}
A userid.  
UserIds are returned from the **/v1/auth** request.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Bearer: A valid authToken
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
If the permission was removed.
{% endapi-method-response-example-description %}

```
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If the connToken provided was not found.
{% endapi-method-response-example-description %}

```
{
    "error": "Not Found"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Execution Endpoints 

Running data commands on a Remote System.

#### General SQL Errors

Errors which occur whilst executing an data command on the host server are passed back as cleanly as possible.    
**Except for the /query endpoint, syntax errors should not occur.**

Errors which might occur include:

* connection string errors \(credentials, host DNS/IPAddress etc\),
* incorrectly named tables and/or incorrectly named fields,
* insufficient permissions to perform task on host,
* creating duplicate records,
* table locks,
* foreign key constraints,
* ... etc ...

Actual error messages depend upon the drivers being used, and the wording of error reporting from the host.  
Generally a 400 error will be generated with a "cleaned" error message in this general JSON format:

```text
{
    "error": "cleaned error message from host system"
}
```

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/query/:driver" %}
{% api-method-summary %}
Execute SQL Statement
{% endapi-method-summary %}

{% api-method-description %}
Runs a command \(or commands\) on the remote system.    
Depending on the command/s, returns a JSON Array \(of Arrays\) of Objects.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter required=true name=":driver" type="string" %}
The driver to use to execute the statement on the remote system.  
At this time, we only have **mssql**.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
A  valid Authentication Token in format:  
   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="token" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter required=true name="statement" type="string" %}
A single statement or command that can be executed on the remote system.  
Multiple statements may be included and should be separated by semi-colons.
{% endapi-method-parameter %}

{% api-method-parameter name="args" type="string" required=false %}
A JSON string containing parameters to be substituted into the **statement** parameter.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a JSON array of arrays of objects.  
Because the statement parameter may be comprised of multiple valid SQL Statements, there is an array returned for each SQL  Statement found. Statements that do not return recordsets \(e.g. delete statements\) will return an empty array. Statements that do return recordsets will return an array of objects where each object is a row from the recordset. 
{% endapi-method-response-example-description %}

```
[
    [],
    [
        {
            "ID": 1,
            "Name": "david",
            ...
        },
        {    
            "ID": 2,
            "Name": "Jim",
            ...
        },
        ...
    ],
    [
        {
            "MyVar": "Good Job!"
        }
    ],
    ...
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Separate errors are reported if fields are missing from the payload.
{% endapi-method-response-example-description %}

```
{ "error": "Missing connection string token" }
{ "error": "Missing payload" }
{ "error": "Missing statement" }
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

####  Note on statement and args \(parameters\)

To allow statements to be dynamic/re-used, the statement field may contain "named tokens" which will be substituted into the statement prior to execution. This creates a kind of stored procedure.

 For Example: 

```javascript
statement: 'SELECT {a} FROM {c} ORDER BY {b};',
args: '{"a": "ID", "b": "CreateDate", "c": "dbo.users"}'
```

Expands out to:

```sql
SELECT ID FROM dbo.users ORDER BY CreateDate;
```

{% hint style="success" %}
**Calling Views**

You can call views using the `/query` endpoint \(and also the `/select` endpoint\).  
Treat a view like a table.
{% endhint %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/exec/:driver" %}
{% api-method-summary %}
Execute Stored Procedure
{% endapi-method-summary %}

{% api-method-description %}
Execute a stored procedure on the remote system.  
**Permission:** ALTER, FULL, ADMIN or OWNER
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name=":driver" type="string" required=true %}
The driver to use to execute the statement on the remote system.   
At this time, we only have **mssql**
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
A valid Authentication Token in format:  
   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connection string token
{% endapi-method-parameter %}

{% api-method-parameter name="procname" type="string" required=true %}
The name of the stored procedure to execute
{% endapi-method-parameter %}

{% api-method-parameter name="params" type="object" required=false %}
An object containing key:value pairs for parameters to be passed into the stored procedure.  
**Note:** Parameters can be declared in any order.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Return recordset or recordsets from the stored procedure - if any.
{% endapi-method-response-example-description %}

```
{
    <results from stored procedure (if any)>
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Separate errors are reported if fields are missing from the payload.
{% endapi-method-response-example-description %}

```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Missing procname in payload" }
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/select/:driver" %}
{% api-method-summary %}
Run Select Query
{% endapi-method-summary %}

{% api-method-description %}
This runs a command on the remote system which is expected to return a paged data set in a JSON  Array of Objects format.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name=":driver" type="string" required=true %}
The driver to use to execute the statement on the remote system.  
At this time, we only have **mssql.**
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
A  valid Authentication Token in format:  
   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connection string Token
{% endapi-method-parameter %}

{% api-method-parameter name="table" type="string" required=true %}
The table to select data from.  
**Note:** Can also be a view name
{% endapi-method-parameter %}

{% api-method-parameter name="fields" required=false type="array" %}
Fields to return. If omitted then all fields will be returned.  
e.g. `[ "ID", "name", "enabled" ]`
{% endapi-method-parameter %}

{% api-method-parameter name="filter" required=false type="array" %}
A JSON array of key/value pair objects containing filtering options for the data to be extracted from the table.  \(see where arrays\)  
e.g. `[ {"ID": 1}, {"enabled": "false"} ]`
{% endapi-method-parameter %}

{% api-method-parameter name="sort" type="array" required=false %}
A JSON string array of fields to sort by. Required if _**limit**_ parameter is provided.  
e.g. `[ "ID DESC", "name" ]`
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="string" required=false %}
Number of results to return in a page.   
If omitted, then defaults to 100 if the _**sort**_ parameter is provided - else all records are returned.
{% endapi-method-parameter %}

{% api-method-parameter name="page" type="string" required=false %}
Page to be returned.   
If omitted then defaults to page 0 \(first page\).  
**Note:** _**Page numbering starts at zero.**_
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a JSON array with each record in the table expressed as a row.
{% endapi-method-response-example-description %}

```
[
    {
        "ID": 1,
        "Name": "david",
        ...
    },
    {    
        "ID": 2,
        "Name": "Jim",
        ...
    },
    ...
]      
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Separate errors are reported if fields are missing from the payload.
{% endapi-method-response-example-description %}

```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Paged query must have sort/order" }
{ "error": "Must have limit and sort if page defined" }
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/insert/:driver" %}
{% api-method-summary %}
Run Insert Query
{% endapi-method-summary %}

{% api-method-description %}
This endpoint creates a new record \(or records\) in the specified table.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name=":driver" type="string" required=true %}
The driver to use to execute the statement on the remote system. At this time, we only have **mssql**.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
A valid Authentication Token in format:  
   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connection string token \(connToken\)
{% endapi-method-parameter %}

{% api-method-parameter name="table" type="string" required=true %}
The table to insert data into
{% endapi-method-parameter %}

{% api-method-parameter name="fields" type="array" required=true %}
An array of fields to add values to. Each array element is a separate record to be added to the table.   
e.g.  `[ "ID", "Name" ]`
{% endapi-method-parameter %}

{% api-method-parameter name="values" type="array" required=true %}
An array of arrays. Each array is a record. Each field in the record array is a value to add.  
**Note:** The order of values must match the order of fields.  
e.g. `[ [ 1, "david" ], [ 2, "mike" ] ]`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a confirmation or else the @@IDENTITY value of the record added.
{% endapi-method-response-example-description %}

```
If a single new record is defined in the values array:
{
    "Identity": N
}
If multiple records defined in the values array:
{
    "result": "success"
}

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Separate errors are reported if fields are missing from the payload.
{% endapi-method-response-example-description %}

```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Insert must have fields defined" }
{ "error": "Insert must have field values defined" }
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/v1/update/:driver" %}
{% api-method-summary %}
Run Update Query
{% endapi-method-summary %}

{% api-method-description %}
This endpoint will update existing records in a table.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name=":driver" type="string" required=true %}
The driver to use to execute the statement on the remote system. At this time, we only have **mssql**.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
A valid authToken in the format:  
"Bearer xxxxx-xxxxxx-xxxxxx
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connection string token \(connToken\)
{% endapi-method-parameter %}

{% api-method-parameter name="table" type="string" required=true %}
The table in which to update data.
{% endapi-method-parameter %}

{% api-method-parameter name="values" type="object" required=true %}
Object containing key:value pairs where the key is the fieldname and the value is the fields value.  
e.g. `{ "name":"david", "address": "my house" }`
{% endapi-method-parameter %}

{% api-method-parameter name="filter" type="array" required=true %}
A JSON array of key/value pair objects containing filtering options for the data to be extracted from the table.  \(see where arrays\)  
e.g. `[ {"ID": 1}, {"enabled": "false"} ]` 
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
If the command executes successfully.  The Updated value \(N\) indicates how many records were actually updated.  
**Note:** _The returned value for Updated should be checked, because if no records match the filter provided then no records will be updated - but a 200 code will still be returned._
{% endapi-method-response-example-description %}

```
{
    "Updated": N
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Separate errors are reported if fields are missing from the payload.
{% endapi-method-response-example-description %}

```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Update must have a filter defined" }
{ "error": "Update must have field values defined" }
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://dbconnector.digital-staging.boston.gov" path="/vi/delete/:driver" %}
{% api-method-summary %}
Run Delete Query
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name=":driver" type="string" required=true %}
The driver to use to execute the statement on the remote system. At this time, we only have **mssql**.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
A valid authToken in the format:  
 "Bearer xxxxx-xxxxxx-xxxxxx"
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="token" type="string" required=true %}
A valid connToken
{% endapi-method-parameter %}

{% api-method-parameter name="table" type="string" required=true %}
The table to delete data from.
{% endapi-method-parameter %}

{% api-method-parameter name="filter" required=true type="array" %}
A JSON array of key/value pair objects containing filtering options for the data to be extracted from the table.  \(see where arrays\)  
e.g. `[ {"ID": 1}, {"enabled": "false"} ]`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
If the command executes successfully.  The Deleted value \(N\) indicates how many records were actually deleted.  
**Note:** _The returned value for Deleted should be checked, because if no records match the filter provided then no records will be deleted - but a 200 code will still be returned._
{% endapi-method-response-example-description %}

```
{
    "Deleted": N
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Separate errors are reported if fields are missing from the payload.
{% endapi-method-response-example-description %}

```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Delete must have a filter defined" }
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### User Permissions

Each user account defined in DBConnector has an assigned role.  

| Role | Role\# | Description |
| :--- | :--- | :--- |
| READ USER \(NORMAL\) | 1 | Can authenticate and then use the **/select** endpoint |
| ALTER USER \(SUPER\) | 2 | Can authenticate and then use the **/select, /update** and  **/insert** endpoints |
| FULL QUERY USER | 4 | Can authenticate and then use the **/select, /update, /insert, /delete** and **/query** endpoints |
| ADMIN | 2048 | Can use all query endpoints and can CRUD users and connections and grant user rights to connections |
| OWNER | 4096 | Can use all endpoints |

Generally, the roles are hierarchical, so for example: a FULL-QUERY-USER can perform all the tasks a READ-USER can perform.

### **Where Arrays**

In the filter fields for the `/select`, `/update` and `/delete` endpoints, the following directives can be used: 

| Filter field shorthand | Meaning |
| :--- | :--- |
| {"username": "david"} | return records where the _username_ is exactly equal to "david". |
| {"**!**username": "david"} | return records where the _username_ is not equal to "david". \(the '!' must be the first char of the string.\) |
| {"username": "david**%**"} | return records where "david" is at the start of the _username_ field. |
| {"username": "**%**david"} | return records where "david" at the end of the _username_ field. |
| {"username": "**%**david**%**"} | return records where "david" is contained in the _username_ field. |
| {"username": **\[**"david", "michael"**\]** | return records where the _username_ is "david" or "michael". |
| {"**^**username": "david"} | return records using an OR join for this filter.  Care as the AND/OR predicates are applied in order they occur in the filter array. |

**Why use an array of objects and not an object?**  
Because some filters might define the same field twice, which would not make sense in an object.   
e.g. Suppose we wanted to run this: `SELECT * FROM MyTable WHERE name ='a' OR name = 'b' or name ='c';`    
To accommodate the filter as an object we would have    
  `{ "name": "a", "^name": "b", "^name": "c"}` which is not a valid structure.  
So we use an array of objects thus:  
   `[ {"name": "a"}, {"^name": "b"}, {"^name": "c"} ]`which is a valid structure.

### Test Data

The endpoint can be tested using some test data pre-loaded into the application.

**The credentials to use are:**

```text
username: devuser
password: Boston2021
```

These credentials can be used with the connToken 

```text
45826BE6-1E29-CC64-B49E-550B9610C2EA
```

**Data contained in testTable available from connStr \#4**

```text
  {
    "ID": 1,
    "Name": "David Upton",
    "Address": "1 Nevermind St, Nowhere",
    "Sex": "Male",
    "Age": 21
  },
  {
    "ID": 2,
    "Name": "Matt McGowan",
    "Address": "14 Glochester Park",
    "Sex": "Male",
    "Age": 21
  },
  {
    "ID": 3,
    "Name": "Phiilip Kelly",
    "Address": "2 Redline, Boston",
    "Sex": "Male",
    "Age": 21
  },
  {
    "ID": 4,
    "Name": "Stella Ubana",
    "Address": "3 Kids is too many, Boston",
    "Sex": "Female",
    "Age": 21
  },
  {
    "ID": 5,
    "Name": "Caroline Stjarnborg",
    "Address": "1 City Hall Plaza, Boston",
    "Sex": "Female",
    "Age": 21
  },
  {
    "ID": 6,
    "Name": "James Duffy",
    "Address": "1 Boston",
    "Sex": "Male",
    "Age": 21
  },
  {
    "ID": 7,
    "Name": "Sebastian Ebard",
    "Address": "2 Nice Street, Rosendale",
    "Sex": "Male",
    "Age": 21
  },
  {
    "ID": 8,
    "Name": "Jeanethe Falvey",
    "Address": "3 Nice Street, Rosendale",
    "Sex": "Female",
    "Age": 21
  }
]
```

