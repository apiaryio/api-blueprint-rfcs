---
RFC: 1
Author: Kyle Fuller
Status: Final
Created: 2015-09-21
Last Modified: 2016-01-26
---

# API Blueprint RFC 1: RFC Template

## Table of Contents

- [What is an RFC](#what-is-an-rfc)
- [RFC Submission Workflow](#rfc-submission-workflow)
    - [Pre-proposal](#pre-proposal)
    - [Submitting draft RFC](#submitting-draft-rfc)
    - [Discussion, development, and updates](#discussion-development-and-updates)
    - [Review & Resolution](#review--resolution)
    - [Implementation](#implementation)

## What is an RFC?

RFC (or Request for Comment) are design documents providing information for new
features within the API Blueprint language. RFCs provide concise technical
specifications of features along with rationales.

RFCs are intended to be the primary mechanism for proposing new features to the
API Blueprint language allowing community input on any issues, and for
documenting design decisions that have gone into the API Blueprint language.

## RFC Submission Workflow

The RFC process looks something like this:

1. [Pre-proposal](#pre-proposal)
2. [Submitting draft RFC](#submitting-draft-rfc) for a change you would like
   to make to the API Blueprint language.
3. [Discussion, development, and updates](#discussion-development-and-updates) -
   The RFC will be discussed, improved, and updated as feedback comes in.
4. [Review & Resolution](#review--resolution) - The RFC is concidered by the API
   Blueprint team and is either accepted or rejected.
5. [Implementation](#implementation) - The implementation of the proposed RFC
   is completed.

### Pre-proposal

The RFC process begins with a new idea or change for API Blueprint. It is
recomended that you reach out to the API Blueprint community to determine if
the proposal would make sense, and to save the author time if the proposal
would be outright rejected for any reason.

Asking the API Blueprint community first if an idea is original help
prevent too much time being spent on something that is guaranteed to be
rejected based on prior discussions (searching the Internet does not always
do the trick).

It also helps to make sure the idea is applicable to the entire community and
not just the author

### Submitting draft RFC

A draft RFC should be presented in the form of a pull request to the API
Blueprint RFC repository.

1. Fork the API Blueprint RFC repository
2. Copy `template.md` to the `rfcs` directory and rename it to to reflect the
   title of the RFC. For example, if the RFC was to propose the ability to
   define headers using MSON, then the title may be "MSON Headers" and the
   filename would become `mson-headers.md`.
3. Fill out the template RFC.
    - Number - Please leave the RFC number blank at this moment, once accepted,
      the RFC will be assigned a number.
    - Motivation - The most critical part of an RFC is to explain why this RFC is
      needed and why existing solutions are indadequate to address the problem that
      the RFC solves. RFC submissions without sufficient motiviation may be
      rejected.
    - Rationale - The rationale fleshes out the specification by describing
      what motivated the design and why particiular design decisions were made.
      It should describe alternate designs that were considered and related
      work.

      The rationale should provide evidence of consensus within the community
      and discuss important objections or concerns raised during discussion.
    - Backwards Compatibility - All RFCs that introduce backwards
      incompatibilities MUST include a section describing these
      incompatibilities and their severity.

### Discussion, development, and updates

At this point there will generally be more discussion, and of course updates
to the RFC.

Updates to a RFC can be submitted as pull requests; once again, a core
developer will merge those pull requests (typically they don't require much if
any review). In cases where the Author has commit access (fairly common), the
Author should just update the draft RFC directly.

### Review & Resolution

Once the author has completed the RFC, the API Blueprint team will review and
finally change the RFCs status to accepted.

For a RFC to be accepted it must meet certain minimum criteria. It must be a
clear and complete description of the proposed change.

### Implementation

Finally, once an RFC has been accepted, the implementation must be completed.
When the implementation is complete and incorporated into the API Blueprint
specification and any related parsers. The status will be changed to "Final".
