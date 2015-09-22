---
RFC: XXXX
Author: Kyle Fuller
Status: Draft
Created: 2015-09-21
Last Modified: 2015-09-21
---

# API Blueprint RFC XXXX: Affordances

## Table of Contents

- [Abstract](#abstract)
- [Rationale](#rationale)
    - [Example](#example)

## Abstract

This RFC proposes that you can define affordances (link relations) that an API
resource or action provides.

## Rationale

This RFC proposes adding an `Affordances` section into API Blueprint that
sits under a resource or action providing the affodances that it provides.

### Example

To describe affordances from a `Question` resource to two other resources, we
can describe them within the affordances section as follows:

```apib
## Question [/question/{id}]

+ Parameters
    + id: 1

+ Affordances
    + `create_comment` (Comments[create])
    + `author` (User)
```

Where `Comment` is a resource named `Comment` that has a `create` relation
identifier and User is another resource.

```apib
## Comments [/comments]
### Create Comment [PUT]
+ Relation: create

## User [/user/{username}]

+ Parameters
    + username
```

