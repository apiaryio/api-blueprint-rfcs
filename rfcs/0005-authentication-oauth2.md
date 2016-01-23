---
RFC: 5
Author: Kyle Fuller
Status: Draft
Created: 2015-09-28
Last Modified: 2015-09-29
---

# API Blueprint RFC 5: OAuth 2 Authentication Scheme

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC adds the "OAuth 2" authentication scheme for the Authentication
framework proposed in RFC 2. OAuth 2 is defined in
[RFC 6749](http://tools.ietf.org/html/rfc6749).

## Motivation

OAuth 2 is a highly common authentication mechanism and it's oftenly used in APIs.

## Rationale

You can use the OAuth 2 Authentication scheme by defining it within the
"Authentication Schemes" section of an API Blueprint.

```apib
### Auth (OAuth 2)
```

### Scopes

Supported scopes may be listed inside the authentication schemes

```apib
### Auth (OAuth 2)

+ scopes
    + user - A user scope, allowing you access to read the user information
    + user.email - A user scope, allowing you access to read the user's email addresses
```

### Endpoints

#### Authorization endpoint

You may describe an authorization endpoint inside the authentication scheme
using the `authorize` relation identifier.

This endpoint represents the the authorization endpoint described in
[RFC6749 section 3.1](http://tools.ietf.org/html/rfc6749#section-3.1).

Inside the authorization endpoint you can describe the parameters that this
endpoint takes along with the parameters it will return during the
`Redirect Callback`.

The `Redirect` keyword has been added to represent the request that will be
made to the `Redirect Callback`.

```apib
#### Authorization [GET /authorize]

+ Relation: authorize
+ Request
    + Parameters
        + `response_type`: code (Response Type, required)
        + `client_id`: b58051708627fa6a5b7e49108f3c7574 (Client ID, required)
        + state (State)
        + scope (Scope)
        + `redirect_uri` (Redirect Callback, required)

+ Redirect
    + Parameters
        + code (Code)
        + state (State)
```

You can describe multiple HTTP transactions within the authorization
endpoint. For example to also describe a `token` `response_type` you
may use the following:

```apib
+ Request
    + Parameters
        + `response_type`: token (Response Type, required)
        + `client_id`: b58051708627fa6a5b7e49108f3c7574 (Client ID, required)
        + `redirect_uri` (Redirect Callback, required)

+ Redirect
    + Parameters
        + `access_token` (Token)
        + `token_type`: Bearer (Token Type)
```

Tools that understand the OAuth 2 authentication scheme should understand the
semantic meaning of the following parameter types within the authorization
endpoint as described in the OAuth 2 specification.

- Response Type
- Client ID
- Client Secret
- State
- Scope
- Redirect Callback
- Token
- Token Type

#### Token endpoint

The token endpoint is used by the client to obtain an access token as
described by [RFC 6749 section 3.2](http://tools.ietf.org/html/rfc6749#section-3.2).
This endpoint is identified by the `token` relation identifier.

API Blueprint tooling can use the types of parameters or attributes to
understand the content being placed within them. Although a user may add
any additonal types of fields as required by the API:

- Grant
- Code
- Redirect Callback
- Client ID
- Client Secret
- Token
- Token Type
- Refresh Token

For example, to describe a token endpoint that uses the `POST` HTTP
method against the `/token` URI you can use the following:

```apib
### Token [POST /token]

+ Relation: token
+ Request
    + Parameters
        + `grant_type`: `authorization_code` (Grant)
        + code (Code)
        + `redirect_uri` (Redirect Callback)
        + `client_id` (Client ID)
        + `client_secret` (Client Secret)

+ Response 200 (application/json)
    + Attributes
        + `access_token` (Token)
```

When a parameter is not described in the URI template for the URI, it is
assumed to be a query parameter.

##### Defining custom request and responses that can return an access token.

You can define any custom flow providing it returns a response including
a access token attribute.

```apib
### Token [POST /token]

+ Relation: token
+ Request (application/json)
    + `grant_type`: password (Grant)
    + username
    + password

+ Response 200 (application/json)
    + Attributes
        + `access_token` (Token)
```

```apib
### Token [POST /token]

+ Relation: token

+ Request (application/x-www-form-urlencoded)
    + Headers:
        + Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW

    + Attributes
        + `grant_type`: `refresh_token`
        + `refresh_token` (Refresh Token)

+ Response 200 (application/json)
    + Attributes
        + `access_token` (Token)
```

#### Revocation Endpoint

You may define an endpoint that can be used to revoke an access token using the
`revoke` relation identifier.

```apib
### Revoke an access token [DELETE /revoke]

+ Relation: revoke
+ Response 204
```
### Access Token Types

API Blueprint tooling may only support the Bearer token type as per described
in this RFC, however any tooling may support other token types defined in
the OAuth specification.

#### Bearer Token

As defined in [RFC6750](http://tools.ietf.org/html/rfc6750), the Bearer token
may be used in many ways.

Inside an OAuth 2 authentication scheme, you can show requests using a Bearer
token under the `Bearer` heading.

```apib
### Bearer

+ Request
    + Headers
        + Authorization (Token)

+ Request (application/x-www-form-urlencoded)
    + `access_token` (Token)

+ Request (application/json)
    + `access_token` (Token)

+ Request
    + Parameters
        + `access_token` (Token)
```

### Referencing an OAuth 2 authentication scheme

When referencing an OAuth 2 scheme from a resource group, resource, action or
example you may specify a comma separated list of OAuth 2 scopes that can be
used.

For example:

```apib
+ Request
    + Authenticated (Auth[user])
+ Response 200 (application/json)
    + Attributes (User)

+ Request
    + Authenticated (Auth[user.email])
+ Response 200 (application/json)
    + Attributes (User)
        + emails (array[Email])
```

