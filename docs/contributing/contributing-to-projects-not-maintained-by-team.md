## Overview {#sec_upstream_contrib_overview}

To comply with the "upstream first" rule and open-source license
requirements, developers collaborate with upstream projects to submit fixes,
improvements, bug reports, investigation results, and other contributions.
Contributions must follow each upstream project's policies and use its preferred
tools, such as mailing lists or GitHub/GitLab pull or merge requests.

## Signing Off Contributions {#sec_upstream_contrib_signoff}

All contributions must be signed off by the developer using their email
account associated with the work's copyright owner, usually a corporate
email address. This requirement does not apply if the upstream project's
policy states otherwise or the project does not support sign-offs. Use a
corporate sender address for email communication.

When a developer contributes code written by someone else, such as a partner,
end user, or third-party contributor, retain the original author's copyright.
Unless the author explicitly requests otherwise, sign off the entire contribution
and add an `Author` field. Use the following Git command:

```text
git commit --signoff --author="Foo Bar <foo.bar@example.com>" -m "comment"
```

By doing this, the developer agrees to the terms of the
[Developer Certificate of Origin](dco-signoff.md#docs_dco).

The developer must make sure that they have rights to submit on behalf
of the original author according to the license and/or author's
permission.

The developer is responsible for checking license compatibility between
the contribution and the upstream project.

## Contribution Agreement {#sec_upstream_contrib_cla}

If the upstream project requires a contribution agreement, the developer must
review it before submitting the contribution. If in doubt, they must contact their manager
or legal team for further guidance.

## Security-related Contribution and Sensitive Data {#sec_upstream_contrib_security}

The developer must verify the data shared with an upstream project to prevent
the disclosure of sensitive information. Pay particular attention to security
issues and issues that could later be classified as security-related. Such cases
must be handled separately according to upstream policy (using private
channels or directly with the Security Officer if upstream has one).
