---
RFC: XXXX
Author: Z
Status: Draft
Created: 2015-09-23
Last Modified: 2015-09-23
---

# API Blueprint RFC XXXX: MSON Parameters

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC proposes using full [MSON][] syntax for description of URI parameters.

## Motivation

Using MSON syntax for description of URI parameters should help unify the
API Blueprint syntax and introduce reusability of URI parameters. In addition it
should solve the following issues:

1. [Usage of data structures in Parameters](https://github.com/apiaryio/api-blueprint/issues/250)
1. [Global query URI parameter](https://github.com/apiaryio/api-blueprint/issues/24)
1. [Description of enumeration elements](https://github.com/apiaryio/api-blueprint/issues/119)
1. [Array in parameters](https://github.com/apiaryio/api-blueprint/issues/240)
1. [Minimum/Maximum values for URI Parameters](https://github.com/apiaryio/api-blueprint/issues/211)

    NOTE: This is going to be addressed once MSON introduces validation.

## Rationale & Implementation

The implementation should assume the existing syntax for API Blueprint
[parameter section][] is – similarly to attributes section – an
[MSON type declaration][].

The type being declared should inherits from Refract
[API Description Namespace][] [Href Variables][] type unless specified
otherwise. If another base type is specified – it has to be of the
`Href Variables` base type.

The `Href Variables` base type should be available as a type for us within the
[data structures section][].

Example:

```apib
# Resource [/resource/{p}{?q}]
- Parameters
    - p
    - q
```

Equals to:

```apib
# Resource [/resource/{p}{?q}]
- Parameters (Href Variables)
    - p
    - q
```

Split previous example for reusability:

```apib
# Resource [/resource/{p}{?q}]
- Parameters (My Parameters)

# Data Structures
## My Parameters (Href Variables)
- p
- q
```

## Considerations

### Name of the Parameters base type

While we can use existing `Href Variables` base type we may also consider using
another type name (that should still be of the `Href Variables` base type).

### Parameters base type constraints

Using any type in as a member type in a descendant of Parameters base
type may lead into problems while representing its value in URL. We may consider
using  `application/x-www-form-urlencoded` together with
[W3C HTML JSON form submission][].

Fore example:

```apib
# Resource [/resource{?q}]
- Parameters
    - q (object)
        - member1: one
        - member2: two
```

Would result into:

```
/resource?p%5Bmember1%5D=one&p%5Bmember2%5D=two
```

Alternatively, parser implementation may consider producing a warning in
situations where member base type may cause an issue.

### Interoperability with other Data Structures

It should be possible to use any MSON data structures – named type – as a member
type of Parameter-based type.

For example:

```apib
# Data Structures

## Date (string)
Date as defined in ISO 8601.

### Sample
2015-09-20

# Resource [/resource{?created}]
- Parameters
    - created (Date)
```

## Backwards Compatibility

Introducing MSON as syntax for description of URI Parameters should be 100%
backward compatible with the existing syntax as it is, in fact, a superset of
syntax currently in use.

---

[MSON]: https://github.com/apiaryio/mson
[parameter section]: https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-uriparameters-section
[MSON type declaration]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#3-type-declaration
[API Description Namespace]: https://github.com/refractproject/refract-spec/blob/master/namespaces/api-description-namespace.md
[Href Variables]: https://github.com/refractproject/refract-spec/blob/master/namespaces/api-description-namespace.md#href-variables-object-type
[data structures section]: https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md#def-data-structures
[W3C HTML JSON form submission]: http://www.w3.org/TR/html-json-forms/#the-application-json-encoding-algorithm
