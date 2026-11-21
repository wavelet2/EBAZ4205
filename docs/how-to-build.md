# How to Build 

## Vivado Project

1. Change directory and launch Vivado GUI.

    ```console
    $ cd ./vivado
    $ <Path-to-Xilinx-tools>/2026.2/Vivado/bin/vivado
    ```

1. Run the following command in the Tcl console to create a Vivado project and generate a block design:

    ```console
    source ./ebaz4205.tcl
    ```

1. Run Implementation from Flow Navigator.  Click OK if prompted to run Synthesis first.

1. Run Generate Bitstream from Flow Navigator.

1. In Vivado, select File -> Export -> Export Hardware

1. In the Export Hardware Platform dialog box, select the Include bitstream check box and export XSA file.

1. Exit Vivado GUI and run the sdtgen tool to convert the XSA file to a System Device Tree for the EDF flow.

    ```console
    $ <Path-to-Xilinx-tools>/2026.2/Vivado/bin/sdtgen
    sdtgen% set_dt_param -dir dts -xsa ebaz4205/ebaz4205_wrapper.xsa
    sdtgen% generate_sdt
    sdtgen% exit
    ```

## AMD EDF Project

1. Download AMD EDF code

    ```console
    $ # Change to the EDF project directory
    $ cd ../yocto/edf
    $ # Use the Git repo tool to download a copy of the 2026.2 AMD EDF repository 
    $ repo init -u https://github.com/Xilinx/yocto-manifests.git -b rel-v2026.2 -m default-edf.xml
    $ repo sync
    $ # Run envoronment setup script.  This script exits inside the 'build' sub-directory
    $ # so move up a directory level after running.
    $ source edf-init-build-env
    $ cd ..
    ```

1. Customise environment

    ```console
    $ # Unpack the tar file with board customisations
    $ tar -xzvf meta-ebaz4205.tgz
    $ # Add the customisations to the flow
    $ bitbake-layers add-layer sources/meta-ebaz4205
    $ # On some new Linux distros, the gen-machine-conf script used in the next step will fail due to AppArmor
    $ # premission issues.  To work around this, temporarily disable AppArmor with the following command:
    $ echo 0 | sudo tee /proc/sys/kernel/apparmor_restrict_unprivileged_userns
    $ # Generate a machine configuration from the System Device Tree
    $ sources/meta-xilinx/gen-machine-conf/gen-machine-conf parse-sdt --hw-description ../../vivado/dts -c build/conf -l build/conf/local.conf --machine-name ebaz4205
    $ # Unpack the tar file with config file customisations
    $ tar -xzvf local.conf.tgz
    ```

1. Build Linux

    ```console
    $ # Start by building the boot.bin file
    $ bitbake xilinx-bootbin
    $ # Build the root file system.
    $ # First, edit local.conf to change variable MACHINE from "ebaz4205" to "amd-cortexa9thf-neon-common"
    $ sed -i 's/ebaz4205/amd-cortexa9thf-neon-common/' build/conf/local.conf 
    $ bitbake edf-linux-disk-image
    ```

    Ignore any warning about "User fwupd-refresh has never been defined"

## microSD card

- The build process results in a OpenEmbedded image file located at build/tmp/deploy/images/amd-cortexa9thf-neon-common/edf-linux-disk-image-amd-cortexa9thf-neon-common.rootfs.wic.  This image file contains 3 partitions:

    ```console
    $ bitbake wic-native
    $ bitbake wic-native -c addto_recipe_sysroot
    $ oe-run-native wic-native wic ls build/tmp/deploy/images/amd-cortexa9thf-neon-common/edf-linux-disk-image-amd-cortexa9thf-neon-common.rootfs.wic
    Getting sysroot...
    Num     Start        End          Size      Fstype
     1             8      2097159   1073741824  fat32
     2       2097160      6291463   2147483648  fat32
     3       6291464      8557663   1160294400  ext4
    ```

    Partition #1 is labeled "esp" and contains the boot.scr file.  It also needs to contain the boot.bin file.  The boot.bin file is located at build/tmp/deploy/images/ebaz4205/boot.bin and needs to be copied to Partition #1 before the image file is flashed to the microSD card.  Partition #2 is labeled "storage" and is empty.  Partition #3 is labeled "root" and contains the root file system.

    ```console
    $ # Copy boot.bin to Partition #1 of image file
    $ oe-run-native wic-native wic cp build/tmp/deploy/images/ebaz4205/boot.bin build/tmp/deploy/images/amd-cortexa9thf-neon-common/edf-linux-disk-image-amd-cortexa9thf-neon-common.rootfs.wic:1
    $ # Flash image file to microSD card
    $ sudo dd if=build/tmp/deploy/images/amd-cortexa9thf-neon-common/edf-linux-disk-image-amd-cortexa9thf-neon-common.rootfs.wic of=/dev/mmcblkX bs=4M
    ```

    The partition labeled "storage" is not needed and can be deleted if desired using parted or gparted.  The root partition can then be expanded to use the remaining microSD card space using gparted.
