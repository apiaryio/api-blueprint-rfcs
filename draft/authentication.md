---
RFC: XXXX
Author: Kyle Fuller
Status: Draft
Created: 2015-09-22
Last Modified: 2015-09-28
---

# API Blueprint RFC XXXX: Authentication Framework

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC proposes an authentication framework that you can use to mark
resource groups, resources, actions or requests as authenticated.
Along with defining a framework for creating authentication schemes.

This RFC does not propose any specific authentication schemes, those are to be
created outside of this RFC.

## Motivation

Being able to express authentication in a machine readable way will allow tools
around API Blueprint to utilise authentication.

## Rationale

### Authentication Scheme Type Definition

Authentication schemes can be defined in an API Blueprint under an
"Authentication Schemes" heading in an API Blueprint. Inside this header
may be a sub-heading which specifies any named authentication types
supported which may be supported by the API.

Users may extend other authentication types to define new authentication
schemes.

For example, to define a named authentication scheme called "Passphrase" which
inherits from another authentication scheme "Basic", we can write the
following in an API Blueprint:

```apib
## Authentication Schemes

### Passphrase (Basic)
```

**NOTE**: *The basic scheme is used as an example in this RFC, and is not
defined within this RFC.*

Under the authentication scheme heading, you may define properties within a
list item. An authentication scheme may provide a set of properties or defaults.

Inside an authentication scheme, you can define responses that may be given
when an authentication scheme is used. For example to indicate the response
that is given when the authentication or permission of the user using
authentication is incorrect.

```apib
### Passphrase (Basic)

+ Response 403 (application/json)

        { "error": "The user does not have access." }
```

## Authenticated Keyword

The `Authenticated` keyword can be used to mark a resource group, resource, or
request example to use authentication.

When the authenticated keyword is used on it's own, it will indicate that all
named authentication schemes defined in the `Authentication Schemes` section
are supported.

For example:

```apib
+ Authenticated
```

You may explictly mention a named or base authentication scheme to specify that
only that scheme is supported.

```apib
+ Authenticated (Basic)
```

You may overide any properties defined in a base or named scheme.

```apib
+ Authenticated (Passphrase)
    + username: katie
```

You may also provide failure responses in anonymous authentication schemes.

```apib
+ Authenticated (Basic)
    + Response 403 (application/json)

            { "error": "The user does not have access." }
```

You specify that multiple authentication schemes are supported, using an
`enum`.

```apib
+ Authenticated (enum)
    + (Basic)
        + username: kyle
        + password: b2952d03bda09cb5f63b0162fbbee77c
    + (Passphrase)
```

### Resource Groups

The authenticated keyword may be used within a resource group to indicate that
all their children resources, actions and example requests inside the
resource group support the specified authentication schemes. Any
specific resource, action, or example requests may overide the inherited
authentication schemes.

Resource groups may be marked as authenticated using the following:

```apib
## Group Questions

+ Authenticated
```

```apib
## Group Questions

+ Authenticated (Basic)
    + username: kyle
    + password: b2952d03bda09cb5f63b0162fbbee77c
```

```apib
+ Authenticated (enum)
    + (Basic)
        + username: kyle
        + password: b2952d03bda09cb5f63b0162fbbee77c
    + (Named)
```

### Resources

Similiary to resource groups, a resource and it's children actions and examples
can be marked as authenticated by placing an `Authenticated` list item.

```apib
## Questions Collection [/questions]

+ Authenticated
```

```apib
## Questions Collection [/questions]

+ Authenticated (Basic)
    + username: kyle
    + password: b2952d03bda09cb5f63b0162fbbee77c
```

### Actions

You can mark a specific action as authenticated using the following:

```apib
### View list of questions [GET]

+ Authenticated
```

```apib
### View list of questions [GET]

+ Authenticated (Basic)
    + username: kyle
    + password: b2952d03bda09cb5f63b0162fbbee77c
```

### Request Examples

Request examples can be marked as authenticated by specifing an authenticated
keyword inside the request as a sub-item.

```apib
+ Request
    + Authenticated
```

```apib
+ Request
    + Authenticated (Basic)
        + username: kyle
        + password: b2952d03bda09cb5f63b0162fbbee77c
```

**NOTE**: *You may not define example responses that an authentication scheme
may return within an anonymous authentication scheme inside a request example.
This limitation is to avoid defining responses within a request to prevent
complexity.*

