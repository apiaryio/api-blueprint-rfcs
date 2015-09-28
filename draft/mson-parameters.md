---
RFC: XXXX
Author: Z
Status: Draft
Created: 2015-09-23
Last Modified: 2015-09-28
---

# API Blueprint RFC XXXX: MSON Parameters and Headers

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Implementation](#implementation)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC proposes using full [MSON][] syntax for description of URI parameters
and HTTP headers.

## Motivation

Using MSON syntax for description of URI parameters and HTTP Headers will
greatly help to unify the API Blueprint syntax and introduce reusability of
URI parameters and HTTP headers. It will also greatly introduce the reusability
of other data structures.

In addition it will address many issues previously tracked on API Blueprint
GitHub repository.

### URI Parameters Issues Tracked

This RFC addresses following issues:

1. [Usage of data structures in Parameters](https://github.com/apiaryio/api-blueprint/issues/250)
1. [Global query URI parameter](https://github.com/apiaryio/api-blueprint/issues/24)
1. [Description of enumeration elements](https://github.com/apiaryio/api-blueprint/issues/119)
1. [Array in parameters](https://github.com/apiaryio/api-blueprint/issues/240)
1. [Minimum/Maximum values for URI Parameters](https://github.com/apiaryio/api-blueprint/issues/211)

    NOTE: This is going to be addressed once MSON introduces validation.

### HTTP Headers Issues Tracked

This RFC addresses following issues:

1. [Parametric Headers Description](https://github.com/apiaryio/api-blueprint/issues/26)
1. [Referenced HTTP headers](https://github.com/apiaryio/api-blueprint/issues/17)
1. [Support for multiline syntax header](https://github.com/apiaryio/api-blueprint/issues/60)

In addition this issue is solved by this RFC:

1. [Reuse headers and attributes](https://github.com/apiaryio/api-blueprint/issues/259)

## Implementation

### Parameters

The implementation should assume the existing syntax for API Blueprint
[URI parameters section][] is, in fact, an [MSON type declaration][].

The type being declared inherits from a *Parameters base type* unless specified
otherwise. If another base type is specified it must inherit from the
*Parameters base type*.

The *Parameter base type* may the [API Description Namespace][]
[`Href Variables`][] type.

The *Parameters base type* should be available as a type for use within any
[data structures section][].

#### Example

```apib
# Resource [/resource/{p}{?q}]
+ Parameters
    + p
    + q
```

Assuming `Parameters` as the name of the *Parameters base type*:

```apib
# Resource [/resource/{p}{?q}]
+ Parameters (My Parameters)

# Data Structures
## My Parameters (Parameters)
+ p
+ q
```

### Headers

The implementation of this RFC should introduce the use of MSON syntax in the
[Headers section][]. Similarly to Parameters section this Headers section should
be considered as [MSON type declaration][].

The type being declared inherits from a *Headers base type* unless specified
otherwise. If another base type is specified it must inherit from the
*Headers base type*.

The *Headers base type* may the [API Description Namespace][] [`HTTP Headers`][]
type.

The *Headers base type* should be available as a type for use within any
[data structures section][].

### Example

```apib
# Resource [/resource]
## Retrieve [GET]
+ Response 200
    + Headers
        + Server: nginx
        + `X-RateLimit-Remaining`: 4987 (number) - Remaining rate limit
```

Assuming `Headers` as the name of the *Headers base type*:

```apib
# Resource [/resource]
## Retrieve [GET]
+ Response 200
    + Headers (My Headers)

# Data Structures
## My Headers
+ Server: nginx
+ `X-RateLimit-Remaining`: 4987 (number) - The rate limit remaining
```

## Considerations

### Parameters base type constraints

Using any type in as a member type in a descendant of Parameters base
type may lead into problems while representing its value in URL. The
implementation should consider using `application/x-www-form-urlencoded`
together with [W3C HTML JSON form submission][] in the case where member type
is of structured type base.

Fore example:

```apib
# Resource [/resource{?q}]
+ Parameters
    + q (object)
        + member1: one
        + member2: two
```

Would result into:

```
/resource?q%5Bmember1%5D=one&p%5Bmember2%5D=two
```

Additionally, a parser implementation may consider producing a warning in
situations where member base type may cause an issue.

### Headers base type constraints

An implementation of a *Headers base type* descendant has to consider there may
be multiple headers of the same name (key) defined. Additionally, if a member
type of an *Haders base type* descendant is of a structured type a certain form
and parser warning should be considered similarly to Parameters base type.

### Interoperability with other Data Structures

It should be possible to use any MSON data structure – named type – as a member
type of Parameters or Headers-based type.

#### Example

```apib
# Resource [/resource{?created}]
+ Parameters
    + created (Date)

## Retrieve [GET]
+ Response 200
    + Headers
        + Date (Date)

# Data Structures

## Date (string)
Date as defined in ISO 8601.

### Sample
2015-09-20
```

## Backwards Compatibility

Introducing MSON as syntax for URI Parameters and HTTP headers description must
be backward compatible with the existing syntax as it is.

[MSON]: https://github.com/apiaryio/mson
[URI parameters section]: https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-uriparameters-section
[Headers section]: https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-headers-section
[MSON type declaration]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#3-type-declaration
[API Description Namespace]: https://github.com/refractproject/refract-spec/blob/master/namespaces/api-description-namespace.md
[Href Variables]: https://github.com/refractproject/refract-spec/blob/master/namespaces/api-description-namespace.md#href-variables-object-type
[data structures section]: https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-data-structures
[W3C HTML JSON form submission]: http://www.w3.org/TR/html-json-forms/#the-application-json-encoding-algorithm
[HTTP Headers]: https://github.com/refractproject/refract-spec/blob/master/namespaces/api-description-namespace.md#http-headers-array-type
