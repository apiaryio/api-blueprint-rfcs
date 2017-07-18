---
RFC: XXXX
Author: Egor Baranov
Status: Draft
Created: 2017-05-03
Last Modified: 2017-05-03
---

# API Blueprint RFC XXXX: Resource prototypes

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC proposes an ability to craete resource prototypes and use it to reduce
code duplication.

## Motivation

When you describe big API several resources can have identical responses.
Good examples of such problem are:

* request format checks - when several requests can return common error like
  `WrongFormat`
* limit rate - when API checks how many requests have user done during some
  period of time and any request can return error like `LimitExceeded`
* authorization checking - some part of API can require authorization so several
  actions can return HTTP 403 forbidden
* deferred actions - when API request don't perform its job synchronously but
  put them into queue and return an identifier back to user; such actions will
  have same response format

All these cases leads to code duplication: you have to write the same response
again and again. This problem can be cause of misspellings and logical errors.
So ability to add common responses and headers in one place for multiple actions
is required.

## Rationale

### Resource prototypes

One of the possible solutions to reduce duplication is creating resource
prototype and setting it to
[resource](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#resource-section)
or
[resource group](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-resourcegroup-section)

We have complicated API with several duplicate responses:

```apib
# Data Structures

## Post

+ title: `test` (string, required)
+ body: `hello world` (string, required)

## User

+ name: `John Smith` (string, required)
+ email: `user@example.com` (string, required)

## Posts [/posts]

### List posts [GET]

+ Response 200
    + Attributes (array[Post], required, fixed-type)

+ Response 429 (application/json)
    + Attributes
        + status: tooManyRequests (string, required, fixed)
        + waitFor: `10` (number, required) - wait before next request

+ Response 400 (application/json)
    + Attributes
        + status: badRequest (string, required, fixed)
        + errors (array[string], required, fixed-type) - list of errors in request

### Show post [GET]

+ Response 200
    + Attributes (Post, required)

+ Response 429 (application/json)
    + Attributes
        + status: tooManyRequests (string, required, fixed)
        + waitFor: `10` (number, required) - wait before next request

+ Response 400 (application/json)
    + Attributes
        + status: badRequest (string, required, fixed)
        + errors (array[string], required, fixed-type) - list of errors in request

## Users [/users]

### List users [GET]

+ Response 200
    + Attributes (array[User], required, fixed-type)

+ Response 429 (application/json)
    + Attributes
        + status: tooManyRequests (string, required, fixed)
        + waitFor: `10` (number, required) - wait before next request

+ Response 400 (application/json)
    + Attributes
        + status: badRequest (string, required, fixed)
        + errors (array[string], required, fixed-type) - list of errors in request

+ Response 403 (application/json)
    + Attributes
        + status: forbidden (string, required, fixed)

### Create user [POST]

+ Response 200
    + Attributes (User, required)

+ Response 429 (application/json)
    + Attributes
        + status: tooManyRequests (string, required, fixed)
        + waitFor: `10` (number, required) - wait before next request

+ Response 400 (application/json)
    + Attributes
        + status: badRequest (string, required, fixed)
        + errors (array[string], required, fixed-type) - list of errors in request

+ Response 403 (application/json)
    + Attributes
        + status: forbidden (string, required, fixed)
```

As you can see responses 429, 400 and 403 are duplicated several times. It is
possible to extract response description into
[Data Structures](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-data-structures)
section and it will help to reduce duplication but you have to write something
like

```apib
+ Response 429 (application/json)
    + Attributes (TooManyRequestsError)
```

for every resource. It is possible to remove duplication completely using new
proposed syntax:

```apib
# Data Structures

## Post

+ title: `test` (string, required)
+ body: `hello world` (string, required)

## User

+ name: `John Smith` (string, required)
+ email: `user@example.com` (string, required)

## Authorized (Resource Prototype)

+ Response 403 (application/json)
    + Attributes
        + status: forbidden (string, required, fixed)

## Common Resource (Resource Prototype)

+ Response 429 (application/json)
    + Attributes
        + status: tooManyRequests (string, required, fixed)
        + waitFor: `10` (number, required) - wait before next request

+ Response 400 (application/json)
    + Attributes
        + status: badRequest (string, required, fixed)
        + errors (array[string], required, fixed-type) - list of errors in request

# Group Blog (Common Resource)

## Posts [/posts]

### List posts [GET]

+ Response 200
    + Attributes (array[Post], required, fixed-type)

### Show post [GET]

+ Response 200
    + Attributes (Post, required)

## Users (Authorized) [/users]

### List users [GET]

+ Response 200
    + Attributes (array[User], required, fixed-type)

### Create user [POST]

+ Response 200
    + Attributes (User, required)
```

In `Data Structures` section two prototypes were defined: `Common Resource`
and `Authorized` with base type `Resource Prototype`.  Both of them have some
responses. To assign prototype for all resources of resource group you need to
use such syntax: `Section name (Prototype Name)`. If section has nested section
then all resources of nested section will have responses defined in prototype.
`Users` resource group will have responces from `Common Resource` prototype
(because it's a prototype of its parent group) and from `Authorized` prototype.

It is possible to assign several prototypes for one resource or resource group:

```apib
# Group Blog (Common Resource, Authorized)
```

It is possible when one prototype has its own prototype:

```apib
# Data Structures

## Common Resource (Rate, Format)
```

## Backwards Compatibility

There will be no backward compatibility problems.
