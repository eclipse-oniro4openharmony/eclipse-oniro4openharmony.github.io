# Building Oniro

Before beginning, install [`git-lfs`](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) and [`repo`](https://gerrit.googlesource.com/git-repo). A full build requires at least 100 GB of free disk space.

## Obtaining the Source Code

To download the source code, execute the following commands in your terminal:

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-6.1-Release -m oniro.xml --no-repo-verify
repo sync -c
repo forall -c 'git lfs pull'
```

Other versions of Oniro are available in the [Oniro manifest repository](https://github.com/eclipse-oniro4openharmony/manifest) by checking the available branches. Some boards may require a specific version, so consult the [Developer Boards](developer-boards/index.md) section to ensure compatibility.

## Setting Up the Build Environment

Use an isolated Docker container to create a clean, controlled build environment. Run the following command to start the container:

```bash
docker run -it -v $(pwd):/home/openharmony swr.cn-south-1.myhuaweicloud.com/openharmony-docker/docker_oh_standard:3.2

```

## Fetching Prebuilt Tools
Once you have the source code, run the following script inside the Docker build environment to fetch the prebuilt tools:

```bash
./build/prebuilts_download.sh
```

## Configuring and Starting the Build

Inside the Docker instance, set the target device for the build (for example, `rk3568`)
and use `ccache` to speed up subsequent builds:

```bash
./build.sh --product-name rk3568 --ccache
```

## Flashing

The flashing procedure is hardware-specific. See the documentation for each device in
[Developer Boards](developer-boards/index.md).

## Additional Tips and Troubleshooting

### HDC Is Not Available on the System

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

To read HiLog output:

```bash
hdc hilog
```

### Speeding Up Build Times

Reduce subsequent build times by mounting directories for prebuilt tools and `ccache` when starting the Docker container. The downloaded tools and compilation cache then persist across builds.

To apply this optimization, use the following command to start your Docker container:

```bash
docker run -it -v $(pwd):/home/openharmony/workdir -v ~/openharmony_prebuilts:/home/openharmony/openharmony_prebuilts -v ~/.ccache:/root/.ccache swr.cn-south-1.myhuaweicloud.com/openharmony-docker/docker_oh_standard:3.2
```

After starting the container with the above command, navigate to the `workdir` directory before initiating the build process:

```bash
cd workdir
```
