# SQL Proxy API (DBConnector)

## Requirement

The City have a number of Data services (e.g. SQL Servers) that reside on the city network.  There is no mechanism by which data can be accessed from these services from processes that are not resident on the Cityhall network.  This includes boston.gov and other cloud based services.

## Service Specification

[Specification May 2021](https://docs.google.com/document/d/1jgzdX7X7V1SbOmna0Uo5TXbVPJnUKTV7NNdHKcNj4so/edit?usp=sharing)

## API User Guide

The `DBConnector` micro-service is available at:

| Environment | API Base URL                                       |
| ----------- | -------------------------------------------------- |
| test        | **https://dbconnector.digital-staging.boston.gov** |
| prod        | **https://dbconnector.boston.gov**                 |

### Flow

#### 1. Authenticate (get or refresh an authToken)

The first step is to post to the **** [**/v1/auth**](sql-proxy-2021.md#authenticate-user) **** endpoint, providing **username** and **password** credentials. &#x20;

* If authenticated, an Authentication Token, and Refresh Token will be returned.
* By default, the Authentication Token (authToken) is valid for 180 seconds, and the Refresh Token for 900 seconds.
* When your account was setup, a different lifetime may have been specified for authTokens issued using your credentials.
* The authToken inherits [role-based permissions](sql-proxy-2021.md#user-permissions) that have been assigned to your credentials.

The authToken is then passed as a "bearer token" in the "Authroization" header - and can be used multiple times until it expires.

**Example request header**

```
HEADER "Authorization: Bearer xxxx-xxxxx-xxxxx"
```

{% hint style="info" %}
When the Authentication Token expires, the Refresh Token can be passed to the **/v1/auth/refresh** endpoint and a new Authentication Token will be returned with a 180 second lifetime. A new Refresh Token will also be generated and returned with a 900 second lifetime. \
_You could also just re-authenticate on the **/v1/auth** endpoint, but using the refresh token is faster and more efficient in the back-end because the user is not re-validated (using the DBConnector database) -saving database connection overhead._
{% endhint %}

#### 2. Fetch or verify connToken (optional)

If you have not saved or been given a connToken (which is a uuid representing a connection string) then the connToken can be obtained from the **/v1/connections/:name** endpoint.

**Note:** You will need to pass the authToken in header.

#### 3. Perform database/remoteAPI operation

Depending on the role attached to the authToken (see [User Permissions](sql-proxy-2021.md#user-permissions)), you can use the **/select, /insert, /update, /delete** and **/query** endpoints to interact with data on the remote host defined by a connToken.

**Notes:** \
1\. You will need to pass the authToken in the "Authorization" header of your request. \
2\. You will need to pass the connToken (and other parameters) in the body/payload of your request

Results from all endpoints will be returned in JSON format.

### Terminology

| Term                            | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **AuthToken**                   | <p>A token which is generated when a user successfully authenticates against the <strong>/v1/auth</strong> endpoint and starts a session. </p><ul><li>This token is used for all subsequent calls to the endpoint during a session.  </li><li>The AuthToken has a lifetime which is typically 180s.  After that the AuthToken expires and needs to be refreshed.</li></ul>                                                                                                                                                                                                                                                                                                                                                                 |
| **RefreshToken**                | <p>A token which can be used to generate a new AuthToken without re-authenticating.  A new AuthToken with a new lifetime of 180s can be generated at <strong>/v1/auth/refresh.</strong> </p><ul><li>The RefreshToken is generated at the same time as the AuthToken and has a lifetime of 900s.  </li><li>After the RefreshToken expires, the only way to generate an AuthToken is to authenticate against the <strong>/v1/auth</strong> endpoint. </li><li>The benefit of the RefreshToken is to ease load on the database server as AuthToken regeneration at <strong>/v1/auth/refresh</strong> endpoint does not require a database request,</li></ul>                                                                                  |
| **User Account**                | A user account is required to authenticate, and is identified by a username and password.  A user account may be only allow connections from a specified IPAddress, and will only be allowed to use certain ConnTokens.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **username/userid**             | Each user account has a unique string and numeric identifier. The username is the string identifier, it must be unique and may be an email address. The userid is a system-generated number which can be used to identify the user in some endpoint operations.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **ConnectionString**            | The `DBConnector` connects to remote (database) environments which are either publicly available, or are housed within the City of Boston network.  To connect to an environment, a connection **** string is required.  Typically the connection string contains the following information about the target: Host, Port, Driver, Credentials.                                                                                                                                                                                                                                                                                                                                                                                             |
| **ConnToken / ConnectionToken** | <p>Each ConnectionString defined within the <code>DBConnector</code> is issued a unique ConnToken when it is saved. Any query requests made via the <code>DBConnector</code> <strong>/v1/query</strong> or <strong>/v1/select</strong> endpoints provide the ConnToken (rather than a Connection String).  </p><ul><li>No Host or Credentials information needs to be stored in the caller system, nor passed across the network by the caller.</li><li>No Host or Credentials are passed across the internet from the caller system.</li><li>If Credentials need to be changed, the change is done once in the <code>DBConnector</code> and all callers will use the new credentials without having to update their ConnTokens.</li></ul> |
| **Session**                     | A session begins when a user authenticates and receives an AuthToken, and ends when the AuthToken expires.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **Calling System**              | The originating application which calls endpoints in this API.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

### Authentication

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/auth" method="post" summary="Authenticate User" %}
{% swagger-description %}
This endpoint is used to initially authenticate the user, and returns an Authentication Token which must be used in the header of all subsequent endpoint calls.

\


\- The Auth Token has a default lifetime of 180 seconds (3 min) (can be set per username),

\


\- The Refresh Token has an additional validity of 180 seconds (3 min).
{% endswagger-description %}

{% swagger-parameter in="body" name="username" type="string" %}
A username (either a name or an email) which is registered in the 

`DBConnector`

 (see /v1/users)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" type="string" %}
The password set for the username provided.
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns an Authentication Token and a Refresh Token, and also the ID of the user, which can/should be used for other endpoint calls." %}
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
{% endswagger-response %}

{% swagger-response status="400" description="Errors determined while authenticating will return one of the following:" %}
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
{% endswagger-response %}

{% swagger-response status="401" description="This will be returned if the user is disabled." %}
```
{"error": "User Disabled"}
```
{% endswagger-response %}

{% swagger-response status="403" description="A 403 may be returned if the request came from an IPAddress not listed for this username, an empty JSON object is returned." %}
```
{}
```
{% endswagger-response %}

{% swagger-response status="404" description="If the username/password does not validate, then an empty JSON object is returned." %}
```
{}
```
{% endswagger-response %}

{% swagger-response status="500" description="If there is an error generating the token, or any other server-side error a 500 status will be returned with an empty JSON string." %}
```
{}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/auth/refresh" method="post" summary="Refresh Authentication Token" %}
{% swagger-description %}
Using a valid Refresh Token (provided from 

**/v1/auth**

 endpoint), this endpoint will refresh a current (or expired) Authentication Token.

\


Also generates a new Refresh Token.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken.  An expired autToken may be used provided it matches the refreshToken and the refreshToken is not expired.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="refresh_token" type="string" %}
A valid refreshToken
{% endswagger-parameter %}

{% swagger-response status="200" description="Successfully generates a new authToken (and also a new refreshToken)." %}
```
*** Normal response
{
    "userid": 6,
    "authToken": "XXXXXX",
    "refreshToken": "XXXXX"
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Other errors that may be returned:" %}
```
*** General Error.
{"error": <explanation>}

*** If no refreshToken was supplied in the body payload.
{"error": "Missing refresh token"}

*** If the refreshToken is not valid.
{"error": "Invalid refresh token"}
```
{% endswagger-response %}

{% swagger-response status="401" description="If there are issues with the authToken, these are the expected messages:" %}
```
*** If no authToken was supplied.
{"error": "Missing Authentication Token"}
*** If the AuthToken supplied has expired.
{"error": "Expired Token"}
*** If the token is unknown or badly formatted.
{"error": "Bad Token"}
```
{% endswagger-response %}

{% swagger-response status="403" description="Usually returned because the callers originating IPAddress does not appear to match the whitelist provided in the user Account." %}
```
{}
```
{% endswagger-response %}
{% endswagger %}

### Common Endpoint Error Messages

The following errors can be raised from calls to the various endpoints.

**400: Bad Request**

400 errors usually indicate some issue with the body or querystring submitted to the endpoint.&#x20;

A JSON string is returned with an _error_ node in it.  The error description is a short explanation of the issue encountered.

```
{
    "error": <description>
}
```

**401: Unauthorised**

Generally, 401 errors are returned when there is an issue with the AuthToken provided in he header.

```
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
* Account has insufficient permissions to perform requested task (see **Permission** description for each endpoint in this guide and also [User Permissions](sql-proxy-2021.md#user-permissions) section)

```
{}
```

**200:OK - Flooding**

If too many requests have been made by a user in a given time period, then the user will be blocked and a 200 response code will be returned with an error message.

```
*** Response if abuse detected
{
    "error": "No Data"
}
```

### User Management

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/users" method="get" summary="List all Users (paged)" %}
{% swagger-description %}
Returns a list of current users.

\




\




**Permission:**

 ADMIN or OWNER.

\




_If the optional `page` & `list` parameters are provided, then a paged output will be returned (sorted by UserID)_
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer - A valid connToken
{% endswagger-parameter %}

{% swagger-parameter in="query" name="page" type="number" %}
The page number to return.

\




**Note:**

 Page numbering starts at zero.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="number" %}
The number of users to return for each page.
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns a JSON Array with user details.
Note: The password field is obfuscated.
(see user permissions for roles)" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/users/:useridentifier" method="get" summary="List a single User" %}
{% swagger-description %}
Returns a single user.

\




\




**Permission:**

 ADMIN, OWNER or the user specified in 

`useridentifier`

.

\


i.e. You can only read your own user record unless you are an ADMIN or OWNER.
{% endswagger-description %}

{% swagger-parameter in="path" name="useridentifier" type="integer" %}
The useridentifier may be either of:

\


\- 

**userID**

 (

_numeric_

): unique user number

\


\- 

**username**

 (

_string_

): unique username
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid connToken.
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns a JSON Array with user details. 
Note: The password field is not obfuscated. 
(see user permissions for roles)" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/users/:useridentifier/connections" method="get" summary="List Connections available to a User" %}
{% swagger-description %}
Provides a list of connection strings that the user has been granted permission to use.

\




\




**Permission:**

 ADMIN, OWNER or user defined by 

`useridentifier`

.

\


i.e. You can only read your own user record unless you are an ADMIN or OWNER.
{% endswagger-description %}

{% swagger-parameter in="path" name="useridentifier" type="string" %}
The useridentifier may be either of:

\


\- 

**userID**

 (

_numeric_

): unique user number

\


\- 

**username**

 (

_string_

): unique username
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken.
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns an array of connection string records which the requested user is permissioned to use." %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/users" method="post" summary="Add a new User" %}
{% swagger-description %}
Adds a new user.

\




\




**Permission:**

 ADMIN or OWNER.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" type="string" %}
Any unique string to identify this user.  Recommended to use email addresses for human users (e.g. "someone@boston.gov") or a meaningful name built around the calling service name (e.g. "cmdb_nightly_update"). 

\


Maximum 100 chars.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" type="string" %}
A complex password. The longer and more complex the better. 
{% endswagger-parameter %}

{% swagger-parameter in="body" name="role" type="number" %}
See User Permissions
{% endswagger-parameter %}

{% swagger-parameter in="body" name="enabled" type="number" %}
1 or 0.  Is this account to be created enabled or disabled. (0=disabled).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ipaddresses" type="string" %}
A comma separated list of IPAddresses the user can make requests from.  If this is left blank, then requests are accepted from all IPAddresses. 

\


Maximum 150 chars.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ttl" type="string" %}
The lifetime of authTokens generated for this user.  If this is left blank, then 180s will be used.  Format is "xxxm/s" (e.g. "90s" for 90 seconds, or "3m" for 3 minutes)

\




**Note:**

 Shorter key lifetimes provide better security.

\


Maxmum 10m or 600s.
{% endswagger-parameter %}

{% swagger-response status="201" description="The ID of the newly created user." %}
```
{
    "id": 16
}
```
{% endswagger-response %}

{% swagger-response status="400" description="As well as the common error messages, an error will be thrown if:
- A duplicate username is attempted to be created.
- The username is more than 100 chars long." %}
```
{
    "error": "Username already exists."
}


{
    "error": "Username is more than 100chars or IPAddress is more than 150 chars or ttl is more than 10 chars."
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/users/:useridentifier" method="patch" summary="Update an existing User" %}
{% swagger-description %}
Updates the specified user with information provided in the payload.

\




\




**Permission:**

 ADMIN or OWNER 
{% endswagger-description %}

{% swagger-parameter in="path" name="useridentifier" type="number" %}
The userid.

\


Userid is returned from the 

**/v1/auth**

 request.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authroization" type="string" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-response status="204" description="If the record was updated." %}
```
{}
```
{% endswagger-response %}

{% swagger-response status="400" description="If no record for the userid is found:" %}
```
{
    "error": "Not Found"
}

*** Tries to rename username with duplicate
 {
     "error": "Username already exists."
 }

```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/users/:userid" method="delete" summary="Delete an existing User" %}
{% swagger-description %}
Deletes the specified user.

\




\




**Permission:**

 ADMIN or OWNER
{% endswagger-description %}

{% swagger-parameter in="path" name="userid" type="number" required="true" %}
The userid (number).

\


Userid is returned from the 

**/v1/auth**

 request.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-response status="204" description="If the record was found and "deleted" (actually will just be disabled)" %}
```
{}
```
{% endswagger-response %}

{% swagger-response status="400" description="If the userid was not found." %}
```
{
    "error": "Not Found"
}
```
{% endswagger-response %}
{% endswagger %}

### Connection Strings

A connection string record contains all the information required for a suitable driver to connect to a remote system.

Each connection string record is defined by a unique UUID (the connToken) -and also a unique name.

{% hint style="info" %}
The connToken is used to refer to the remote system in [execution endpoints](sql-proxy-2021.md#execute-commands-on-remote-system) so that connectivity details do not need to be stored in and passed from the calling system.
{% endhint %}

{% hint style="success" %}
The connToken (UUID) should never change once the connections string record is created, and therefore can be safely stored in the calling system.
{% endhint %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connections" method="get" summary="List all Connections (paged)" %}
{% swagger-description %}
Returns a list of all active remote system connection strings.

\




\




**Permission:**

 ADMIN OR OWNER
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/:token" method="get" summary="List single Connection" %}
{% swagger-description %}
Returns the remote system connection string defined by the specified Connection Token.

\




\




**Permission:**

 ADMIN, OWNER or by a user who has permission to use the connection.
{% endswagger-description %}

{% swagger-parameter in="path" name="token" type="string" %}
Bearer: A valid authToken.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="If the requested connToken is not found in the database." %}
```
{ "error": "connToken not found" }
{ "error": "malformed connToken" }
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/find/:name" method="get" summary="Find connToken" %}
{% swagger-description %}
Fetch a connection token using the tokens name.

\




\




**Permission:**

 All authenticated users.
{% endswagger-description %}

{% swagger-parameter in="path" name=":name" type="string" %}
The name of a token
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connection/:token/users" method="get" summary="List Users who may use a Connection" %}
{% swagger-description %}
Provides a list of users who have been granted permission to use a connection string.

\




\




**Permission:**

 ADMIN or OWNER
{% endswagger-description %}

{% swagger-parameter in="path" name="token" type="string" %}
Bearer: A valid connToken.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connection" method="post" summary="Add a new Connection" %}
{% swagger-description %}
Saves a connection string.

\




**Permission:**

 ADMIN or OWNER.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="connectionString" type="string" %}
The connection string, usually as a JSON string.

\




`"{`

\


  

`\"host\":\"somewhere.com\",`

 

\


  

`\"username\":\"sa\",`

\


  

`\"password\": \"asdfasd\"`

\


`}"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="string" %}
A name by which this connection string can be easily referred to.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
The purpose of the connection string.

\




**Tip:**

 Include the driver and/or type of connection defined.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="enabled" type="number" %}
1 (enabled) or 0 (disabled).

\


Defaults to 1 (enabled) 
{% endswagger-parameter %}

{% swagger-response status="201" description="Returns a message with the newly created connToken." %}
```
{
    "connToken": "12342343412341"
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Errors related to the parameters passed, or caused by insert action." %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connection/:token/user/:userid" method="post" summary="Grant User Permission to Connection" %}
{% swagger-description %}
Grants a user permission to use a connection string.

\




\




**Permission:**

 FULL/SUPER, ADMIN or OWNER.
{% endswagger-description %}

{% swagger-parameter in="path" name="token" type="string" %}
A valid connToken
{% endswagger-parameter %}

{% swagger-parameter in="path" name="userid" type="integer" %}
A userid (number).

\


(The userid is returned from the 

**/v1/auth**

 request)
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: a valid authToken
{% endswagger-parameter %}

{% swagger-response status="201" description="If the permission is granted." %}
```
{}
```
{% endswagger-response %}

{% swagger-response status="400" description="Errors related to the payload or actual insertion of the permission" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/:token" method="patch" summary="Update an existing Connection" %}
{% swagger-description %}
Updates the remote system connection string defined by the specified Connection Token. 

\




\




**Permission**

: ADMIN or OWNER..
{% endswagger-description %}

{% swagger-parameter in="path" name="token" type="string" %}
A valid connToken.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-parameter in="body" name="connectionString" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="enabled" type="integer" %}

{% endswagger-parameter %}

{% swagger-response status="204" description="If the connection is successfully updated." %}
```
{}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connections/:token" method="delete" summary="Delete an existing Connection" %}
{% swagger-description %}
Deletes the remote system connection string defined by the specified Connection Token.

\




\




**Permission:**

 ADMIN or OWNER
{% endswagger-description %}

{% swagger-parameter in="path" name="token" type="string" %}
A valid connToken
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-response status="204" description="The connection string is disabled." %}
```
{}
```
{% endswagger-response %}

{% swagger-response status="400" description="If the connToken provided cannot be found." %}
```
*** Connection string was not found in system.
{
    "error": "Not Found"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/connection/:token/user/:userid" method="delete" summary="Revoke User Permission for Connection" %}
{% swagger-description %}
Revoke an existing permission for a user to use a connection string.

\




\




**Permission:**

 ADMIN or OWNER
{% endswagger-description %}

{% swagger-parameter in="path" name="token" type="string" %}
A valid connToken.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="userid" type="integer" %}
A userid.

\


UserIds are returned from the 

**/v1/auth**

 request.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Bearer: A valid authToken
{% endswagger-parameter %}

{% swagger-response status="204" description="If the permission was removed." %}
```
{}
```
{% endswagger-response %}

{% swagger-response status="400" description="If the connToken provided was not found." %}
```
{
    "error": "Not Found"
}
```
{% endswagger-response %}
{% endswagger %}

### Execution Endpoints&#x20;

Running data commands on a Remote System.

#### General SQL Errors

Errors which occur whilst executing an data command on the host server are passed back as cleanly as possible.  \
**Except for the /query endpoint, syntax errors should not occur.**

Errors which might occur include:

* connection string errors (credentials, host DNS/IPAddress etc),
* incorrectly named tables and/or incorrectly named fields,
* insufficient permissions to perform task on host,
* creating duplicate records,
* table locks,
* foreign key constraints,
* ... etc ...

Actual error messages depend upon the drivers being used, and the wording of error reporting from the host.\
Generally a 400 error will be generated with a "cleaned" error message in this general JSON format:

```
{
    "error": "cleaned error message from host system"
}
```

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/query/:driver" method="post" summary="Execute SQL Statement" %}
{% swagger-description %}
Runs a command (or commands) on the remote system.  

\


Depending on the command/s, returns a JSON Array (of Arrays) of Objects.
{% endswagger-description %}

{% swagger-parameter in="path" name=":driver" type="string" %}
The driver to use to execute the statement on the remote system.

\


At this time, we only have 

**mssql**

.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
A  valid Authentication Token in format:

\


   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="statement" type="string" %}
A single statement or command that can be executed on the remote system.

\


Multiple statements may be included and should be separated by semi-colons.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="args" type="string" %}
A JSON string containing parameters to be substituted into the 

**statement**

 parameter.
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns a JSON array of arrays of objects.
Because the statement parameter may be comprised of multiple valid SQL Statements, there is an array returned for each SQL  Statement found. Statements that do not return recordsets (e.g. delete statements) will return an empty array. Statements that do return recordsets will return an array of objects where each object is a row from the recordset. " %}
```
*** If the statement provided contains a single sql statement.
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

*** If the statement provided contains multiple sql statements.
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
{% endswagger-response %}

{% swagger-response status="400" description="Separate errors are reported if fields are missing from the payload." %}
```
{ "error": "Missing connection string token" }
{ "error": "Missing payload" }
{ "error": "Missing statement" }
```
{% endswagger-response %}
{% endswagger %}

#### &#x20;Note on statement and args (parameters)

To allow statements to be dynamic/re-used, the statement field may contain "named tokens" which will be substituted into the statement prior to execution. This creates a kind of stored procedure.

&#x20;For Example:&#x20;

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

You can call views using the `/query` endpoint (and also the `/select` endpoint).\
Treat a view like a table.
{% endhint %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/exec/:driver" method="post" summary="Execute Stored Procedure" %}
{% swagger-description %}
Execute a stored procedure on the remote system.

\




**Permission:**

 ALTER, FULL, ADMIN or OWNER
{% endswagger-description %}

{% swagger-parameter in="path" name=":driver" type="string" %}
The driver to use to execute the statement on the remote system. 

\


At this time, we only have 

**mssql**
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
A valid Authentication Token in format:

\


   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" %}
A valid connection string token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="procname" type="string" %}
The name of the procedure to execute
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params" type="object" %}
An object containing key:value pairs for parameters to be passed into the stored procedure.

\




**Note**

: Input parameters can be declared in any order
{% endswagger-parameter %}

{% swagger-parameter in="body" name="output" type="object" %}
An object containing name:type pairs for output parameters to be passed into the stored procedure.

\


The type must be one of the following strings: "number" or "varchar" 

\




**Note:**

 Ouput parameters can be declared in any order.
{% endswagger-parameter %}

{% swagger-response status="200" description="Return recordset or recordsets from the stored procedure - if any." %}
```
*** If the stored procedure contains a single sql statement.
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
*** If the stored procedure contains multiple statements
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

*** If the stored procedure executed contains a single sql statement 
    and output variables.
{
    result: [
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
    output: {
        param1: "value",
        param2: "value",
        ...
    }
}
*** If the stored procedure executed contains multiple sql statements 
    and output variables.
{
    result: [
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
    ],
    output: {
        param1: "value",
        param2: "value",
        ...
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Separate errors are reported if fields are missing from the payload." %}
```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Missing procname in payload" }
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/select/:driver" method="post" summary="Run Select Query" %}
{% swagger-description %}
This runs a command on the remote system which is expected to return a paged data set in a JSON  Array of Objects format.
{% endswagger-description %}

{% swagger-parameter in="path" name=":driver" type="string" %}
The driver to use to execute the statement on the remote system.

\


At this time, we only have 

**mssql.**
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
A  valid Authentication Token in format:

\


   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" %}
A valid connection string Token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="table" type="string" %}
The table to select data from.

\




**Note:**

 Can also be a view name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fields" type="array" %}
Fields to return. If omitted then all fields will be returned.

\


e.g. 

`[ "ID", "name", "enabled" ]`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="array" %}
A JSON array of key/value pair objects containing filtering options for the data to be extracted from the table.  (see where arrays)

\


e.g. 

`[ {"ID": 1}, {"enabled": "false"} ]`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sort" type="array" %}
A JSON string array of fields to sort by. Required if 

_**limit**_

 parameter is provided.

\


e.g. 

`[ "ID DESC", "name" ]`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="limit" type="string" %}
Number of results to return in a page. 

\


If omitted, then defaults to 100 if the 

_**sort**_

 parameter is provided - else all records are returned.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" type="string" %}
Page to be returned. 

\


If omitted then defaults to page 0 (first page).

\




**Note: **

_**Page numbering starts at zero.**_
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns a JSON array with each record in the table expressed as a row." %}
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
{% endswagger-response %}

{% swagger-response status="400" description="Separate errors are reported if fields are missing from the payload." %}
```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Paged query must have sort/order" }
{ "error": "Must have limit and sort if page defined" }
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/insert/:driver" method="post" summary="Run Insert Query" %}
{% swagger-description %}
This endpoint creates a new record (or records) in the specified table.
{% endswagger-description %}

{% swagger-parameter in="path" name=":driver" type="string" %}
The driver to use to execute the statement on the remote system. At this time, we only have 

**mssql**

.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
A valid Authentication Token in format:

\


   "Bearer xxxxx-xxxxxx-xxxxxx"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" %}
A valid connection string token (connToken)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="table" type="string" %}
The table to insert data into
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fields" type="array" %}
An array of fields to add values to. Each array element is a separate record to be added to the table. 

\


e.g.  

`[ "ID", "Name" ]`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="values" type="array" %}
An array of arrays. Each array is a record. Each field in the record array is a value to add.

\




**Note:**

 The order of values must match the order of fields.

\


e.g. 

`[ [ 1, "david" ], [ 2, "mike" ] ]`
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns a confirmation or else the @@IDENTITY value of the record added." %}
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
{% endswagger-response %}

{% swagger-response status="400" description="Separate errors are reported if fields are missing from the payload." %}
```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Insert must have fields defined" }
{ "error": "Insert must have field values defined" }
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/v1/update/:driver" method="post" summary="Run Update Query" %}
{% swagger-description %}
This endpoint will update existing records in a table.
{% endswagger-description %}

{% swagger-parameter in="path" name=":driver" type="string" %}
The driver to use to execute the statement on the remote system. At this time, we only have 

**mssql**

.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
A valid authToken in the format:

\


"Bearer xxxxx-xxxxxx-xxxxxx
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" %}
A valid connection string token (connToken)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="table" type="string" %}
The table in which to update data.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="values" type="object" %}
Object containing key:value pairs where the key is the fieldname and the value is the fields value.

\


e.g. 

`{ "name":"david", "address": "my house" }`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="array" %}
A JSON array of key/value pair objects containing filtering options for the data to be extracted from the table.  (see where arrays)

\


e.g. 

`[ {"ID": 1}, {"enabled": "false"} ]`

 
{% endswagger-parameter %}

{% swagger-response status="200" description="If the command executes successfully.  The Updated value (N) indicates how many records were actually updated.
Note: The returned value for Updated should be checked, because if no records match the filter provided then no records will be updated - but a 200 code will still be returned." %}
```
{
    "Updated": N
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Separate errors are reported if fields are missing from the payload." %}
```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Update must have a filter defined" }
{ "error": "Update must have field values defined" }
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://dbconnector.digital-staging.boston.gov" path="/vi/delete/:driver" method="post" summary="Run Delete Query" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name=":driver" type="string" %}
The driver to use to execute the statement on the remote system. At this time, we only have 

**mssql**

.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
A valid authToken in the format:

\


 "Bearer xxxxx-xxxxxx-xxxxxx"
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="string" %}
A valid connToken
{% endswagger-parameter %}

{% swagger-parameter in="body" name="table" type="string" %}
The table to delete data from.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="array" %}
A JSON array of key/value pair objects containing filtering options for the data to be extracted from the table.  (see where arrays)

\


e.g. 

`[ {"ID": 1}, {"enabled": "false"} ]`
{% endswagger-parameter %}

{% swagger-response status="200" description="If the command executes successfully.  The Deleted value (N) indicates how many records were actually deleted.
Note: The returned value for Deleted should be checked, because if no records match the filter provided then no records will be deleted - but a 200 code will still be returned." %}
```
{
    "Deleted": N
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Separate errors are reported if fields are missing from the payload." %}
```
{ "error": "Missing connection string token" }
{ "error": "connToken not found" }
{ "error": "Missing payload" }
{ "error": "Missing table in payload" }
{ "error": "Delete must have a filter defined" }
```
{% endswagger-response %}
{% endswagger %}

### User Permissions

Each user account defined in DBConnector has an assigned role. &#x20;

| Role               | Role# | Description                                                                                         |
| ------------------ | ----- | --------------------------------------------------------------------------------------------------- |
| READ USER (NORMAL) | 1     | Can authenticate and then use the **/select** endpoint                                              |
| ALTER USER (SUPER) | 2     | Can authenticate and then use the **/select, /update** and  **/insert** endpoints                   |
| FULL QUERY USER    | 4     | Can authenticate and then use the **/select, /update, /insert, /delete** and **/query** endpoints   |
| ADMIN              | 2048  | Can use all query endpoints and can CRUD users and connections and grant user rights to connections |
| OWNER              | 4096  | Can use all endpoints                                                                               |

Generally, the roles are hierarchical, so for example: a FULL-QUERY-USER can perform all the tasks a READ-USER can perform.

### **Where Arrays**

In the filter fields for the `/select`, `/update` and `/delete` endpoints, the following directives can be used:&#x20;

| Filter field shorthand                     | Meaning                                                                                                                              |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| {"username": "david"}                      | return records where the _username_ is exactly equal to "david".                                                                     |
| {"**!**username": "david"}                 | return records where the _username_ is not equal to "david". (the '!' must be the first char of the string.)                         |
| {"username": "david**%**"}                 | return records where "david" is at the start of the _username_ field.                                                                |
| {"username": "**%**david"}                 | return records where "david" at the end of the _username_ field.                                                                     |
| {"username": "**%**david**%**"}            | return records where "david" is contained in the _username_ field.                                                                   |
| {"username": **\[**"david", "michael"**]** | return records where the _username_ is "david" or "michael".                                                                         |
| {"**^**username": "david"}                 | return records using an OR join for this filter.  Care as the AND/OR predicates are applied in order they occur in the filter array. |

**Why use an array of objects and not an object?**\
****Because some filters might define the same field twice, which would not make sense in an object. \
e.g. Suppose we wanted to run this: `SELECT * FROM MyTable WHERE name ='a' OR name = 'b' or name ='c';` \
To accommodate the filter as an object we would have  \
&#x20; `{ "name": "a", "^name": "b", "^name": "c"}` which is not a valid structure.\
So we use an array of objects thus:\
&#x20; `[ {"name": "a"}, {"^name": "b"}, {"^name": "c"} ]`which is a valid structure.

### Test Data

The endpoint can be tested using some test data pre-loaded into the application.

**The credentials to use are:**

```
username: devuser
password: Boston2021
```

These credentials can be used with the connToken&#x20;

```
45826BE6-1E29-CC64-B49E-550B9610C2EA
```

**Data contained in testTable available from connStr #4**

```
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
