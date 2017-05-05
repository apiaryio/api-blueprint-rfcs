---
RFC: XXXX
Author: Egor Baranov
Status: Draft
Created: 2017-05-03
Last Modified: 2017-05-03
---

# API Blueprint RFC XXXX: Common Data

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC proposes an ability to add common responses and to multiple actions.

## Motivation

When you describe big API several actions can have identical responses.
For example you have 10 actions allowed for authorized user only, so each
of such actions can respond with 401. If you have some complex response you can
create named type and use it 10 times but even in this situation you have to
write same code 10 times. This problem connected not only authorization but
with other tasks. If you have common error in several actions you will have
exatly the same situation. If you want to change this code you have to change it
in 10 places. This problem can be cause of misspellings and logical errors.
So ability to add common responses in one place for multiple actions is
required.

## Rationale

### Common Data

Common responses can be defined in an API Blueprint under an "Common Data" header.
Inside this header one or several Response sections are allowed.
Here is an example:

```apib
# Group Authorized resources

## Common Data

+ Response 401

## Posts [GET /posts]

+ Response 200

## Comments [GET /comments]

+ Response 200
```

This code equals to:

```apib
# Group Authorized resources

## Posts [GET /posts]

+ Response 200

+ Response 401

## Comments [GET /comments]

+ Response 200

+ Response 401
```

It is possible to have several "Common Data" sections:

```apib
# Group Authorized resources

## Common Data

+ Response 401

## Common Data

+ Response 500

## Posts [GET /posts]

+ Response 200

## Comments [GET /comments]

+ Response 200
```

This code equals to:

```apib
# Group Authorized resources

## Posts [GET /posts]

+ Response 200

+ Response 401

+ Response 500

## Comments [GET /comments]
+ Response 200

+ Response 401

+ Response 500
```

Common Data section influences to current actions group and all internal actions
groups, but not neighbors:

```apib
# Group Authorized resources

## Common Data

+ Response 401

## Posts [GET /posts]

+ Response 200

# Group Authorization

## Auth [POST /auth]

+ Response 200
```

This code equals to:

```apib
# Group Authorized resources

## Posts [GET /posts]

+ Response 200

+ Response 401

# Group Authorization

## Auth [POST /auth]

+ Response 200
```

`/auth` action has no 401 response, because it is located in separate group.

### Action prototypes

Another variant of solving problem is using actions inheritance. In this
situation we creates base action and all actions of group inherites its
responses:

```apib
# Group Authorized resources

# Prototype

+ Response 401

## Posts [GET /posts]

+ Response 200
```

In this example we have created unnamed prototype for actions group Authorized
resources. In this groups and all its subgroups all actions will inherit
responses from the prototype.

Code above equals to:

```apib
# Group Authorized resources

## Posts [GET /posts]

+ Response 200

+ Response 401
```

We can set several prototypes for one group:

```apib
# Group Authorized resources

# Prototype

+ Response 401

## Prototype

+ Response 500

## Posts [GET /posts]

+ Response 200
```

We can created named prototype. Advantage of this variant is an ability to
disable prototype for some actions group:

```apib
## Prototype Authorized

+ Response 401

After previous line all actions will have 401 response

# Group Authorized resources

## Posts [GET /posts]

+ Response 200

# Group Authorization

## Unuse Prototype Authorized

In this section there will be no 401 response because we have diabled prototype
Authorized

## Auth [POST /auth]

+ Response 200
```

## Backwards Compatibility

If anyone use "Common Data" or "Prototype" headers in the documentation after
this RFC it will treat as special section instead of regular markdown header.
If there is no such section in the documention there will be no backward
compatibility problems.
