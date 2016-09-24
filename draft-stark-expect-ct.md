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

Web hosts that serve the Expect-CT HTTP header are noted by the UA as Expect-CT
hosts. The UA evaluates each connection to an Expect-CT host for compliance with
the UA's Certificate Transparency (CT) policy. If the connection violates the CT
policy, the UA sends a report to a URI configured by the Expect-CT host and/or
fails the connection, depending on the configuration that the Expect-CT host has
chosen.

Deploying safely with reporting and gradually increasing max-age. Risk of UAs
changing policies, servers should use reporting to discover.

Meant to be used with HSTS but they can be used separately.

Expect-CT is a trust-on-first-use (TOFU) mechanism etc etc

## Requirements Language

# Server and Client Behavior

## Response Header Field Syntax

The "Expect-CT" header field is a new response header defined in this
specification. It is used by a server to indicate that UAs should evaluate
connections to the host emitting the header for CT compliance
({{expect-ct-compliance}}).

(I don't know how to do fancy ABNF figures.)

~~~
Expect-CT-Directives = directive *( OWS ";" OWS directive )
directive            = directive-name [ "=" directive-value ]
directive-name       = token
directive-value      = token / quoted-string
~~~

The directives defined in this specification are described below. The overall
requirements for directives are:

1. The order of appearance of directives is not significant.

2. A given directive MUST NOT appear more than once in a given header
   field. Directives are either optional or required, as stipulated in their
   definitions.

3.  Directive names are case insensitive.

4.  UAs MUST ignore any header fields containing directives, or other header
    field value data, that do not conform to the syntax defined in this
    specification.  In particular, UAs must not attempt to fix malformed header
    fields.

5.  If a header field contains any directive(s) the UA does not recognize, the
    UA MUST ignore those directives.

6.  If the Expect-CT header field otherwise satisfies the above requirements (1
    through 5), the UA MUST process the directives it recognizes.

### The report-uri Directive

The OPTIONAL `report-uri` directive indicates the URI to which the UA SHOULD
report Expect-CT failures ({{expect-ct-compliance}}). The UA POSTs the reports
to the given URI as described in {{reporting-expect-ct-failure}}.

Hosts may set `report-uri`s that use HTTP or HTTPS. If the scheme in the
`report-uri` is one that uses TLS (e.g., HTTPS), UAs MUST check Expect-CT
compliance when the host in the `report-uri` is an Expect-CT host; similarly,
UAs MUST apply HSTS if the host in the `report-uri` is a Known HSTS Host.

Note that the report-uri need not necessarily be in the same Internet
domain or web origin as the host being reported about.

UAs SHOULD make their best effort to report Expect-CT failures to the
`report-uri`, but they may fail to report in exceptional conditions.  For
example, if connecting the `report-uri` itself incurs an Expect-CT failure or
other certificate validation failure, the UA MUST cancel the connection.
Similarly, if Expect-CT Host A sets a `report-uri` referring to Expect-CT Host
B, and if B sets a `report-uri` referring to A, and if both hosts fail to comply
to the UA's CT policy, the UA SHOULD detect and break the loop by failing to
send reports to and about those hosts.

UAs SHOULD limit the rate at which they send reports. For example, it is
unnecessary to send the same report to the same `report-uri` more than once.

### The enforce Directive

The OPTIONAL `enforce` directive is a valueless directive that, if present
(i.e., it is "asserted"), signals to the UA that compliance to the CT policy
should be enforced (rather than report-only) and that the UA should refuse
future connections that violate its CT policy.

### The max-age Directive

The `max-age` directive specifies the number of seconds after the reception of
the Expect-CT header field during which the UA SHOULD regard the host from whom
the message was received as an Expect-CT host.

The `max-age` directive is REQUIRED to be present within an "Expect-CT" header
field if and only if the `enforce` directive is present. The `max-age` directive
is meaningless if no `enforce` directive is present (i.e., if the Expect-CT
policy is report-only). UAs MUST ignore the `max-age` directive if the `enforce`
directive is not present and not cache the header.

(syntax of max-age goes here)

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
