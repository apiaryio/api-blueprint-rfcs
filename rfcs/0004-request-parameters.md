---
RFC: 0004
Author: Kyle Fuller
Status: Accepted
Created: 2015-11-20
Last Modified: 2015-11-30
---

# API Blueprint RFC 0004: Request Parameters

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

These changes propose a way to attach different URI Parameter values
to different transaction examples, allowing you to provide alternative
responses based on matched URI Parameters.

## Motivation

These changes will allow users to express how responses may be different when a
requesting client sends different URI parameter values.

Requests with different parameters will also allow tooling around API Blueprint
such as testing tools and mocking tools to provide better matching of responses
based on different request parameters.

## Rationale

This proposal will add the ability to attach parameters to a request. The
parameters attached to a request with override and not inherit from the
previous parameter definition.

```apib
### Search for a question [GET /questions{?query,limit}]

+ Parameters
    + query: language - Search query
    + limit: 20 (number, optional) - Maximum number of questions returned

+ Response 200 (application/json)

        [
            { "question": "Favourite programming language?" },
            { "question": "API Description language preference" }
        ]

+ Request A search limited to one result
    + Parameters
        + query: language
        + limit: 1

+ Response 200 (application/json)

        [
            { "question": "Favourite programming language?" }
        ]
```
