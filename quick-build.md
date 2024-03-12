---
layout: default
title: Building Oniro
parent: Eclipse Oniro Project
---

# Building Oniro

Before beginning, ensure that [`git-lfs`](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) and [`repo`](https://gerrit.googlesource.com/git-repo) are installed. It is recommended to have at least 100GB of free disk space available for the full build.

## Obtaining the Source Code

To download the source code, execute the following commands in your terminal:

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-4.0-Release --no-repo-verify
repo sync -c
repo forall -c 'git lfs pull'
```

## Fetching Prebuilt Tools

Once you have the source code run the following script to fetch the prebuilt tools:

```bash
./build/prebuilts_download.sh
```

## Setting Up the Build Environment

For building the project, using an isolated Docker container is recommended for a clean and controlled build environment. Run the following command to start the Docker container:

```bash
docker run -it -v $(pwd):/home/openharmony swr.cn-south-1.myhuaweicloud.com/openharmony-docker/docker_oh_standard:3.2

```

## Configuring and Starting the Build

Inside the Docker instance, set the target device for the build (e.g. rk3568)
and use ccache to speed up subsequent builds:

```bash
./build.sh --product-name rk3568 --ccache
```

## Flashing

The flashing procedure is highly hardware specific and can be found in the
[Developer Boards](dev-boards.html) section for each individual device.

## Additional Tips and Troubleshooting

### No HDC available in the system

If the `hdc` tool is not available on your host system, build it using the `ohos-sdk`:

```bash
./build.sh --product-name ohos-sdk --ccache
```

Find the `hdc` tool in `out/sdk/ohos-sdk/linux/toolchains`. To verify the connection with the device, run:

```bash
$ hdc list targets
150100424a544434520325874bb44900
```

For sending commands to the device:

```bash
hdc shell
```

To read hilog output:

```bash
hdc hilog
```

### Speeding Up Build Times

You can significantly reduce build times for subsequent builds by mounting directories for prebuilts and ccache when initiating the Docker container. This approach ensures that once the prebuilts are downloaded, they don't need to be fetched again, and the compilation cache is maintained across builds.

To apply this optimization, use the following command to start your Docker container:

```bash
docker run -it -v $(pwd):/home/openharmony/workdir -v ~/openharmony_prebuilts:/home/openharmony/openharmony_prebuilts -v ~/.ccache:/root/.ccache swr.cn-south-1.myhuaweicloud.com/openharmony-docker/docker_oh_standard:3.2
```

After starting the container with the above command, navigate to the `workdir` directory before initiating the build process:

```bash
cd workdir
```