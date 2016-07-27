---
RFC: XXXX
Author: Kyle Fuller
Status: Draft
Created: 2015-02-04
Last Modified: 2015-02-04
---

# API Blueprint RFC XXXX: Semantic Versioning

## Table of Contents

- [Abstract](#abstract)
- [Motivation](#motivation)
- [Rationale](#rationale)
- [Backwards Compatibility](#backwards-compatibility)

## Abstract

This RFC proposes that the API Blueprint language and specification are
versioned via [Semantic Versioning](http://semver.org), and there is a
[CHANGELOG](https://github.com/kylef/changelog) file including a list of
changes for each release.

## Motivation

Currently API Blueprint uses a non-standard version format such as `1A9` which
is vastly different from version numbers elsewhere. The benefits of semantic
versioning is that anyone can understand the types of changes in major, minor
or patch releases, while that isn't possible using our current release
versioning.

Semantic versioning would bring API Blueprint's versioning to the same format
as used by the wide range of tooling and parsers for API Blueprint.

## Backwards Compatibility

This is entirely backwards compatible, since the previous released versions are
the same and this only affects upcoming versions.
