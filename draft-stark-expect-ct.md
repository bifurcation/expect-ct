---
title: "Expect-CT (better title TBD)"
abbrev: "Expect-CT"
docname: draft-stark-expect-ct.md
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: E. Stark
    name: Emily Stark
    org: Google
    email: estark@google.com

--- abstract

TODO: Wite an abstract here

--- middle

# Introduction

This document defines a new HTTP header that enables UAs to identify web hosts
that wish to require the presence of Signed Certificate Timestamps (SCTs) in
future Transport Layer Security (TLS) {{!RFC5246}} connections.

Deploying safely with reporting and gradually increasing max-age. Risk of UAs
changing policies, servers should use reporting to discover.

Meant to be used with HSTS but they can be used separately.

Expect-CT is a trust-on-first-use (TOFU) mechanism.

## Requirements Language

# Server and Client Behavior

## Response Header Field Syntax

The "Expect-CT" header field is a new response header defined in this
specification. It is used by a server to indicate that UAs should evaluate
connections to the host emitting the header for CT compliance
({{expect-ct-compliance}}).

## Server Processing Model

## User Agent Processing Model

## Noting Expect-CT

## Evaluating Expect-CT Connections for CT Compliance {#expect-ct-compliance}

# Reporting Expect-CT Failure

# Security Considerations

# Privacy Considerations

# IANA Considerations

# Usability Considerations

When the UA detects an Expect-CT host in violation of the UA's CT policy, users
will experience denials of service. It is advisable for UAs to explain the
reason why.

It is advisable that UAs have a way for users to clear noted Expect-CT hosts and
that UAs allow users to query noted Expect-CT hosts.
