---
layout: default
title: OpenHarmony Mirror
parent: Eclipse Oniro Project
---

# OpenHarmony Mirror

The Eclipse Oniro project offers a complete GitHub-hosted
[mirror](https://github.com/eclipse-oniro-mirrors/) of the OpenHarmony project,
originally hosted at [Gitee](https://gitee.com/openharmony).
One-way synchronization of changes from Gitee to GitHub is performed once a day.

The reason for this mirror is the increased speed and reliability it offers
to the Eclipse Oniro project and its developers. We encourage using it to avoid
disconnects during sync and slow download speeds for developers working outside of China.
In addition to speed and reliability it also allows for streamlining the process
of forking and merging within GitHub and its large user base.

To ensure these mirrors are consistently up-to-date, a
[synchronization CI workflow](https://github.com/eclipse-oniro-mirrors/mirror-sync)
is executed every 24 hours to incorporate the latest changes from Gitee into the
existing repositories, as well as mirroring newly created repositories.

For those involved in active development or wishing to contribute, please visit
the [Eclipse Oniro for OpenHarmony ](https://github.com/eclipse-oniro4openharmony)
GitHub organization. This is where a fork of the manifest and newly created add-ons
are being developed. The mirror itself is read-only and no changes from GitHub will
be merged into it.
