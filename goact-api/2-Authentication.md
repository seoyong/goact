# Authorization and authentication

 goAct API utilizes the OAuth2 as authentication protocol. In addition to
standard OAuth2 username/password authentication, backend provides the ability
to authenticate as an application. Authentication information must be sent to
backend within every request, in the Authorization HTTP header.

You must never store  goAct user's username or password in your application.
Instead, you exchange them to the access token, which is used to authenticate
all requests.

## Token types

Backend supports two token types: application and user tokens.

### 1. Application tokens

Application authentication is used for requests that do not require user
authentication, for example creating a new user. Token is provided in the
HTTP authorization header in following format:

```
Authorization: Application <application id>:<application secret>
```

### 2. User tokens

The user token is granted by backend to particular user using the flow described
below. Token is provided in the authorization header in following format:

```
Authorization: UserToken <user token>
```

## User authentication: Mobile flow

This flow is used in a case where the end user has trusted her password with the
client app. It conforms to OAuth2 specification section
http://tools.ietf.org/html/rfc6749#section-4.3

### 1. POST /mint/api/v1/auth/authorize

application/json field | Value
----------|------
grant_type | password
username | _User's username, typically the email address_
password | _User's password_

**Authentication**

No authentication headers are required.

**Example Request**

```sh
curl -i -H "Content-Type: application/json" -X POST -d '{"grant_type" : "password", "username":"kc@goact.com.au","password":"xyz"}' https://test.goact.co/mint/api/v1/auth/authorize
```

**Example Response**

```javascript
{ 
    "user" : 1231,
    "access_token" : "2YotnFZFEjr1zCsicMWpAA",
    "token_type" : "user",
    "expires_in" : 3600
}
```

Property | Meaning
------|-------- 
response | Error Message 
user | Id of the associated user
access_token | The access token to be used in Authorization header of the requests
token_type | The token type, currently supported type is "user"
expires_in | *Optional* In how many seconds the token expires. If missing, the token is not set to expire.

If the token is already expired, or otherwise cancelled, HTTP 403 error is
returned. In this case, the client application should ask the user to
authenticate again.


**Errors**

Error identifier | HTTP Status | Description
-----------------|-------------|------------
400 | 400 | The requested resource was not found on server 
400 | 404 | The password does not match, suggest reset? 

 

## Checking access token information

### 1. GET /mint/api/v1/auth/token_info

Get information about the access token used in the request.

**Authentication**

Request must be authenticated by User specific token.

**Example Request**

```sh
curl -i -H "Content-Type: application/json" -H "Authorization: UserToken 2YotnFZFEjr1zCsicMWpAA" -X GET https://test.goact.co/mint/api/v1/auth/token_info
```

**Example Response**

```javascript
{
  "user" : 1231,
  "token_type": "user"
}
```

Please see previous section for explanations of the access token properties.

 
**Errors**

Error identifier | HTTP Status | Description
-----------------|-------------|------------
400 | 400 | The requested resource was not found on server  














## User registration: Mobile flow

This flow is used in a case when the end user sign up with the client app. 

### 1. POST /mint/api/v1/auth/register

application/json field | Value
----------|------
grant_type | signup
firstname | _User's first name_
lastname | _User's last name_
username | _User's username, typically the email address_
password | _User's password, passwords length should be more than 5 characters_

**Authentication**

Request must be authenticated by Application specific token.

**Example Request**

```sh
curl -i -H "Content-Type: application/json" -X POST -d '{"grant_type" : "signup", "firstname":"Kate", "lastname":"Smith", "username":"kc@goact.com.au","password":"xyz"}' https://test.goact.co/mint/api/v1/auth/authorize
```

**Example Response**

```javascript
{ 
    "user" : 1231,
    "access_token" : "2YotnFZFEjr1zCsicMWpAA",
    "token_type" : "user",
    "expires_in" : 3600
}
```

Property | Meaning
------|-------- 
response | Error Message 
user | Id of the associated user
access_token | The access token to be used in Authorization header of the requests
token_type | The token type, currently supported type is "user"
expires_in | *Optional* In how many seconds the token expires. If missing, the token is not set to expire.

If the username has already existed, HTTP 403 error is
returned. In this case, the client application should ask the user to
sign-up with different username again or log in with same username.


**Errors**

Error identifier | HTTP Status | Description
-----------------|-------------|------------
403 | 403 | The username has already existed

 
