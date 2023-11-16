---
title: Eclipse Oniro4OpenHarmony
layout: home
---
## Eclipse Oniro4OpenHarmony Documentation

Oniro is an Eclipse Foundation project focused on developing a distributed open source operating system platform that enables interoperability of devices, regardless of brand, make, or model. The platform is designed to be compatible with a broad range of embedded operating system environments, including OpenHarmony, an open source operating system specified and hosted by the OpenAtom Foundation. Managed as an Eclipse Foundation project and working group, Oniro benefits from the Eclipse Foundation's extensive experience in open source governance, along with an advanced IP compliance and licensing toolchain.

Oniro provides the foundational fabric for a wide range of devices, both large and small, offering seamless interoperability, modularization, and a rich graphical user interface. It supports various global technologies and use cases across industries, including Consumer Electronics, Home Appliances, Industrial IoT devices, Smart Homes, and Multimedia.

----

## Quick Start

As prerequisites git-lfs and repo need to be installed. 100GB of free disk space
is recommended for the full build.

**To obtain the source code use the following commands:**

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-3.2-Release --no-repo-verify
repo sync -c
repo forall -c 'git lfs pull'
```

**In the source code directory, fetch the prebuild tools:**

```bash
./build/prebuilts_download.sh
```

**To run the build an isolated docker container is recommended:**

```bash
docker run -it -v $(pwd):/home/openharmony swr.cn-south-1.myhuaweicloud.com/openharmony-docker/openharmony-docker:1.0.0

```

**In the Docker instance, run the build:**

Select the target device with (e.g.rk3568):

```bash
hb set
```

Start the build:

```bash
hb build
```

