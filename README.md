# second-loader

It's a init script for the [OpenWrt](https://openwrt.org/) operating system that loads a kernel from external storage
device.

## How it works

The `build` script builds a special OpenWrt image with a custom init script called `second-loader`.
This image is installed into the internal memory of device as a regular OpenWrt image.
The custom init script looks for OpenWrt kernel on external storage devices.
If successful, it loads the kernel using `kexec` and makes the partition from which the kernel was loaded the root file
system.
Otherwise, it calls the default `init` program.
Thus, this OpenWrt image looks like powerful bootloader from external storage devices.

## Building and installation

1. Download and install a [OpenWrt SDK](https://openwrt.org/docs/guide-developer/using_the_sdk) or build it manually.
2. Optionally configure a future OpenWrt image using the `make menuconfig` and `make kernel_menuconfig` commands.
3. Run the `build` script and pass it the path to the OpenWrt SDK.
4. Install OpenWrt image to the device.
5. Make a partition on the the external storage device and format it to the ext4 filesystem.
6. Optionally reconfigure and rebuild the OpenWrt image for the external storage device.
7. Copy the contents of the OpenWrt image to the external storage device.
8. Copy the kernel of OpenWrt to the `<external storage device>/boot/vmlinuz` file.

## Configuring the `second-loader` init script

The `/etc/second-root` configuration file contains a UUID of the target partition.
If this file is empty or the `second-loader` script can't find a partition with UUID from this file
it calls the default `init` program.
You can find out the UUID of a partition from your storage device using the `blkid` command.
