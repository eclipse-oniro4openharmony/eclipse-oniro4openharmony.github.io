---
layout: default
title: OpenHarmony Downstream/Upstream Relationship
parent: Eclipse Oniro Project
---
# OpenHarmony Downstream/Upstream Relationship

## Eclipse Oniro Downstream Integration

Eclipse Oniro currently bases its enhancements of OpenHarmony on the 6.0 release.
The project will target newer versions as they become available and working group
members adopt them. It uses a downstream fork to collect enhancements and fixes,
check their consistency, and run them through continuous integration (CI).

The downstream fork consists of a forked
[manifest](https://github.com/eclipse-oniro4openharmony/manifest) and forks of
the changed components. The manifest references all new or forked components for
inclusion in the build. Use a different URL in `repo init` so that `repo sync`
obtains the correct repositories and revisions from the manifest.

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-6.1-Release --no-repo-verify
```

Creating a new repository is necessary for new add-ons. Once the add-on is
pushed, it can be referenced in the forked manifest and included in the CI
build.

We also encourage developers, when possible, to submit fixes
and enhancements into OpenHarmony's GitCode repositories. These changes
will return to Eclipse Oniro in the next release or earlier if backported
to the current release branches on GitCode.

## Upstreaming into OpenHarmony

As a downstream OpenHarmony distribution, Eclipse Oniro focuses on well-integrated,
well-tested features for and from its partners. The downstream fork holds changes
that developers and CI test before release.

The fork is not intended to store most changes permanently.
Any modification applicable to OpenHarmony upstream should be proposed 
through a pull request to the GitCode master branch. This provides feedback from
OpenHarmony developers and helps the changes fit the existing codebase. Changes
accepted upstream will return to Eclipse Oniro with the next
release synchronization, reducing the maintenance burden to carry patches downstream.

When a fix is integrated upstream, backport it to the current release so that it is
included in the active release cycle.
