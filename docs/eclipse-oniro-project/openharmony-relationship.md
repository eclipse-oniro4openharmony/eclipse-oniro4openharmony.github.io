---
layout: default
title: OpenHarmony Downstream / Upstream relationship
parent: Eclipse Oniro Project
---
# OpenHarmony Downstream / Upstream relationship

## Eclipse Oniro Downstream Integration

Eclipse Oniro currently bases its enhancements of OpenHarmony on the 4.1 release.
Newer versions will be targeted as they become available and used by the
working group members. To collect enhancements and fixes, check for consistency, and 
run them through continuous integration (CI), the project is using a downstream fork to 
incorporate these changes.

The downstream fork consists of a forked
[manifest](https://github.com/eclipse-oniro4openharmony/manifest) and forks of
the changed components. All newly created or forked components will be referenced 
in the manifest for inclusion in the build. The only change required would be to use a 
different URL in `repo init`, enabling repo sync to pick up the correct repositories 
and revisions from the manifest.

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-4.1-Release --no-repo-verify
```

Creating a new repository is necessary for new add-ons. Once the add-on is
pushed, it can be referenced in the forked manifest and included in the CI
build.

We also encourage every developer, if possible, to directly upstream fixes
and enhancements into OpenHarmony's Gitee repositories. These changes
will return to Eclipse Oniro with the next release, or earlier if backported
to the current release branches on Gitee.

## Upstreaming into OpenHarmony

For Eclipse Oniro, a downstream OpenHarmony distribution, the primary focus is to 
ensure  well integrated and tested features from and for partners. The
downstream fork will hold all changes and will be tested by developers and CI for
releases.

However, this fork isn't intended as a permanent repository for most changes. 
Any modification applicable to OpenHarmony upstream should be proposed 
via a pull request on the Gitee master branch. By doing so we get crucial
feedback from OpenHarmony developers. This will help in the design and
implementation of changes, to fit well into the existing code base. Additionally,
any changes viable for upstream inclusion will return to Eclipse Oniro with the next
release synchronization, reducing the maintenance burden to carry patches downstream.

Should a fix be integrated upstream, efforts should be directed towards backporting 
it into the current release. This ensures its inclusion in our ongoing cycle.
