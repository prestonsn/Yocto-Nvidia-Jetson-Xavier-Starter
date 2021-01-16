# Yocto starter project for Nvidia Jetson Xavier

Instructions for creating a linux image for the Xavier. The latest releases for
Yocto are listed on this website: https://wiki.yoctoproject.org/wiki/Releases. 



## Install Dependencies

Download the Yocto-Poky foundation layer, version `Gatesgarth` is the latest
when writing this.

```bash
git clone git://git.yoctoproject.org/poky.git -b gatesgarth
```

Now download the tegra layer.

```bash
cd poky
git clone https://github.com/madisongh/meta-tegra.git -b gatesgarth
```

## Compile Source

Initialize the build environment.

```bash
cd poky
source oe-init-build-env
```

The above command creates a `conf/local.conf` file that specifies the target 
machine. Open this file.

```bash
cd poky/build/conf
xdg-open local.conf
```

Now append the following options into the file.

```bash
MACHINE = "jetson-xavier"
IMAGE_CLASSES += "image_types_tegra"
INHERIT += "rm_work"
IMAGE_FSTYPES = "tegraflash tar.gz"
```

Next modify `bblayers.conf` inorder to load the provided recipes. The first 
three lines of the BB (bitbake) layer list should look similar, now add the 
last line in order to use the tegra layer. `PATH` on your system will 
be different, use the one already present in the bblayers.conf.

```
BBLAYERS ?= " \
/PATH/poky/meta \
/PATH/poky/meta-poky \
/PATH/poky/meta-yocto-bsp \
\
/PATH/poky/meta-tegra \
"
```

## Build Image

Now that that build environment is configured, our architecture and
layer locations specified, we can build the image

```bash
bitbake core-image-minimal
```
If you indent to use the image with Nvidia's dev tools, you can build the image
with the following command.

```bash
bitbake code-image-sato-dev
```
