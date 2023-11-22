---
title: REUSE Compliance
parent: Contribution
layout: default
---

# SPDX Information and REUSE Standard

All projects and files for an hosted project **MUST** be
[REUSE](https://reuse.software/) compliant. REUSE requires SPDX
information for each file, rules for which are as follows:

- for files copyrighted by projects contributors (**\"First Party
  Files\"**):
  - any new file MUST have a SPDX header (copyright and license);
  - for files that don\'t support headers (for example binaries,
    patches etc.) an associated `.license` file MUST be included
    with the relevant SPDX information;
  - do not add Copyright Year as part of the SPDX header
    information;
  - the general rule for patch files is to use the MIT license and
    _not_ the license of the component for which the patch applies -
    the latter solution would be error-prone and hard to manage and
    maintain in the long run, and there may be difficult-to-handle
    cases (what if the patches modifies multiple files in the same
    component - eg. gcc - which are subject to different licenses);
  - when modifying a file through this contribution process, you may
    (but don\'t have to) claim copyright by adding a copyright line;
  - you MUST NOT alter copyright statements made by others, but only
    add your own;
- for files copyrighted by third parties and just added to the project
  by contributors, eg. files copied from other projects or back-ported
  patches (**\"Third Party Files\"**):
  - if upstream files already have SPDX headers, they MUST be left
    unchanged;
  - if upstream files do _not_ have SPDX headers:
    - the exact upstream provenance (repo, revision, path) MUST be
      identified;
    - you MUST NOT add SPDX headers to Third Party Files;
    - copyright and license information, as well as upstream
      provenance information (in the \"Comment\" section), MUST be
      stored in [.reuse/dep5]{.title-ref} following [Debian dep5
      specification](https://dep-team.pages.debian.net/deps/dep5/)
      (see examples below);
    - you MUST NOT use wildcards (\*) in dep5 \"Files\" paragraphs
      even if Debian specs allow it: it may lead to unnoticed
      errors or inconsistencies in case of future file additions
      that may be covered by wildcard expressions even if they
      have a different license;
    - in case of doubts or problems in finding the correct license
      and copyright information for Third Party Files,
      contributors may ask the project\'s Legal Team in the
      project mailing list <oniro-dev@eclipse.org>;

## SPDX Header Example

Make sure all of your submitted new files have a licensing statement in
the headers. Please make sure that the license for your file is
consistent with the licensing choice at project level and that you
select the correct SPDX identifier, as in the following example for
Apache 2.0 license:

```text
/*
 * SPDX-FileCopyrightText: Jane Doe <jane@example.com>
 *
 * SPDX-License-Identifier: Apache-2.0
 */
```

## DEP5 \"Files\" Paragraph Examples

```text
Files: meta-oniro-staging/recipes-containers/buildah/buildah_git.bb
Copyright: OpenEmbedded Contributors
License: MIT
Comment: Recipe file for buildah copied from meta-virtualization project at
         https://git.yoctoproject.org/meta-virtualization,
         recipes-containers/buildah.
         README file of meta-virtualization project states:
         "All metadata is MIT licensed unless otherwise stated."

Files: meta-oniro-staging/recipes-devtools/ninja/ninja/0001-feat-support-cpu-limit-by-cgroups-on-linux.patch
Copyright: Google Inc.
License: Apache-2.0
Comment: Patch for ninja backported from Ninja project at
        https://github.com/ninja-build/ninja, commit 540be33
        Copyright text left as found in the header of the patched file.
```

## Substantial Contributions

Therefore, if your contribution is only a patch directly applied to an
existing file, then you are not required to do anything. If your
contribution is an entire new project, or a substantial, copyrighted
contribution, you **MUST** make sure that you do that following the [IP
Policy](https://git.ostc-eu.org/oss-compliance/ip-policy/) and that you
comply with REUSE standard to include the licensing information where
they are required.
