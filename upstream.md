---
layout: default
title: OpenHarmony Downstream / Upstream relationship
parent: Eclipse Oniro Project
---
# OpenHarmony Downstream / Upstream relationship

## Eclipse Oniro Downstream Integration

Eclipse Oniro currently bases its enhancements of OpenHarmony on the 4.0 release.
Newer versions will be targeted as they become available, and being used by the
working group members. To collect enhancements and fixes, check for consistency and
run them through CI the project is using a downstream fork to land these changes.

The downstream fork does consist of a forked
[manifest](https://github.com/eclipse-oniro4openharmony/manifest) and forks of
the changed components. All newly created or forked components will be referenced
from the manifest to be included into the build. Using a different URL in `repo init`
would be the only change to allow repo sync pick up the right repositories and
revisions from the manifest.

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-4.0-Release --no-repo-verify
```

For new add-ons a new repository would need to be created. Once the add-on is
pushed it can be referenced from the forked manifest and included into the CI
build.

We also encourage every developer to directly upstream fixes and enhancements
into into the Gitee repositories from OpenHarmony, if possible. These changes
will come back to Eclipse Oniro with the next release, or earlier, if backported
in the current release branches on Gitee.

## Upstreaming into OpenHarmony

For Eclipse Oniro as a downstream OpenHarmony distribution the focus is on
ensuring well integrated and tested features from and for partners. The
downstream fork will hold all changes and will be tested by developers and CI for
releases.

This fork is no long-term home for most of the changes, though.
Any change that is also applicable to OpenHarmony upstream we need to propose a
pull request against the master branch in Gitee. By doing so we get crucial
feedback from OpenHarmony developers. This will help in the design and
implementation of changes to fit well into the existing code base. And secondly,
any change that can be upstreamed will come back to Eclipse Oniro with the next
release sync and reduce the maintenance burden to carry patches downstream.

In case a fix was upstreamed we should also work on backporting it into the
current release to allow us getting it back into our flow in the current cycle.
