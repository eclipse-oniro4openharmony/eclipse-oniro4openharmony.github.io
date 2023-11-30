---
title: Contributing to Projects not Maintained by Team
parent: Contributing
nav_order: 7
layout: default
---

# Overview {#sec_upstream_contrib_overview}

In order to comply with \"upstream first\" rule and Open Source licenses
requirements, developers collaborate with several upstream projects to
submit fixes, improvements, bug reports, problem investigation results
etc. Contribution must be made in accordance with upstream project
policy using the tooling upstream project prefers such as mailing list,
github/gitlab pull/merge requests, etc.

# Signing-off Contribution {#sec_upstream_contrib_signoff}

All contributions must be signed off by the developer using their email
account associated with the copyright owner of the work (in most cases
it will be the corporate email address). This does not apply if the
upstream project policy says otherwise or signing off of the
contribution is not possible due to upstream project\'s limitation. It
is recommended to use corporate email address as a sender address in
case of email communication.

In case the developer contributes code written by someone else (provided
by partner, end user, third-party contributor etc) original author\'s
copyright must be kept and entire contribution must be signed off with
\"Author:\" tag unless the author explicitly asks otherwise. This could
be done in the `git` submission:

```text
git commit --signoff --author="Foo Bar <foo.bar@example.com>" -m "comment"
```

By doing this developer states that they agree to the terms of
`DCO<docs_dco>`{.interpreted-text role="ref"}

The developer must make sure that they have rights to submit on behalf
of the original author according to the license and/or author\'s
permission.

It is developer\'s responsibility to check license compatibility between
the contribution and the upstream project.

# Contribution Agreement {#sec_upstream_contrib_cla}

In case the upstream project requires signing of contribution agreement
of any kind, the developer must review it carefully before submitting
the contribution. In case of any doubt they must contact their manager
or legal team for further guidance.

# Security-related Contribution and Sensitive Data {#sec_upstream_contrib_security}

It is the developer\'s responsibility to verify the data they share with
upstream counterpart to prevent leak of sensitive information. Special
attention must be given in the case of security issues or issues which
can be potentially rated as security-related in the future. Such cases
must be handled separately according to upstream policy (using private
channels or directly with the Security Officer if upstream has one).
