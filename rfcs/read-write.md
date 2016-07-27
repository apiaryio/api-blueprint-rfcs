---
RFC: XXXX
Author: Phil Sturgeon
Status: Draft
Created: 2016-06-06
Last Modified: 2016-06-06
---

# API Blueprint RFC XXXX: Readable & Writable Attributes

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC proposes the addition of keywords `readable` and `writable`, to mark
if a particular attribute is read-only, write-only, or by default: both.

In a very similar fashion to `required` and `optional`, these options would go
into the attribute definition:

``` md
+ created: 1415203908 (number, readable) - Time stamp
```

For this example, `created` is a server-generated field which will not come
from the client, and trying to set it could either do nothing (ignoring the
value) or error, entirely at the will of the server implementation.

## Motivation

Whilst attributes in resources should be as close to identical on write and read
as possible, they are often not. Sometimes attributes will be displayed, but
if you try to modify them in a `POST`, `PUT` or `PATCH` they will be ignored.

Sometimes a value might be available to write, but will not be presented back to
the client in a `GET` action, or in the response body. This is more rare for
REST APIs, but it does still happen.

Communicating when fields are read-only or write-only in the existing
documentation structure can be difficult.

Clients can end up sending fields which are not being saved, and if the server
is not so strict as to error, it will simply be black-holing that data.

## Backwards Compatibility

No breaks here, it just adds another pair of words to the list.
