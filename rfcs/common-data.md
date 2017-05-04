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
of such actions can respond with 401. That is why you have to write same code
10 times. If you want to change this code you have to change it in 10 places.
This problem can be cause of misspellings and logical errors.
So ability to add common responses in one place for multiple actions is required.

## Rationale

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

## Backwards Compatibility

If anyone use "Common Data" header in the documentation after this RFC it will
treat as special section instead of regular markdown header. If there is no
such section in the documention there will be no backward compatibility problems.
