---
RFC: 0003
Author: Kyle Fuller
Status: Draft
Created: 2015-09-22
Last Modified: 2015-09-28
---

# API Blueprint RFC 0003: Basic Authentication Scheme

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC adds the "Basic" authentication scheme for the Authentication
framework proposed in RFC 0002.

## Motivation

Basic authentication is a commonly used authentication mechanism, and is part
of the HTTP/1.1 specification defined in
[RFC1945 section 11.1](http://tools.ietf.org/html/rfc1945#section-11.1).

## Rationale

The "Basic" authentication scheme is based on the model that the user
agent must authenticate itself with a user-ID and a password.

As such, a basic authentication scheme may configure two properties,
`username` and `password`. These properties indicate a sample username and
password that may be used.

For example, as a named authentication scheme:

```apib
### Auth (Basic)

+ username: kyle
+ password: b2952d03bda09cb5f63b0162fbbee77c
```

As an anonymous scheme:

```apib
+ Authenticated (Basic)
    + username: kyle
    + password: b2952d03bda09cb5f63b0162fbbee77c
```

There are no default values for the sample `username` and `password`. When
these are both unset, there is no sample credentials.

Implementations of the Basic Authentication Scheme may provide sufficient
warnings or errors if a `username` or `password` is configured without
the counterpart.
