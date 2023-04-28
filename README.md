# 4.6.2_evm_topaz2m
Jetson Nano driver for OPTIMOM 2M



# Driver specifications
Feature
Value
Driver Version
0.5
Sensor
TOPAZ2M
Resolutions
1920x1080@65FPS (RAW10)
1920x1080@100FPS (RAW8)
1920x800@80FPS (RAW10)
1920x800@130FPS (RAW8)
Colorspace format
Monochrome:
RAW8
RAW10
Pixel Depth
8-10 bit
Interface
x2 MIPI Lanes
Controls
Analog gain
Digital gain
Exposure time
Frame rate
Test pattern
Flip
Lastest JetPack
4.6.2 for Jetson Nano Devkit
Supported Boards
Nvidia Jetson Nano Dev Kit A02
Nvidia Jetson Nano Dev Kit B01 (x2 camera ports)



This driver is given as an example to help customers to design their software application. The full functionality and robustness is not guaranty
The driver code can be modified and adapted to the target platform without restriction.
Teledyne-e2v gives support to install and use this driver example but is not responsible in case the driver is not responsible in case of diver failure.



A fast way to start the Development Kit with a new installation is to use a Massflash image. Such image can be sent upon request: ask to your local support. 
Please refer to the part ?Install the driver/Massflash? for more detail.



# Driver history

Version
Date
Description
0.5
2023/03/15
* Support for two Topaz2m sensors: CAM0 and CAM1
* Modifies the minimum exposure value from 1 to 5
0.4.2
2023/03/02
* Modifies the analog gain control range from 1-16 to 1-15
0.4.1
2023/12/22
* Correct Topaz chip ID test that blocked the startup of some devices
0.4
2022/10/04
* This driver version supports the following controls:
o Analog gain
o Digital gain
o Exposure
o Frame rate
o Flip
o Test pattern
0.3
2022/09/28
* support to the Topaz2M driver sensor in the Jetson Nano A02 
0.2
2022/08/30
* Add instructions to test new modes:
o 1920x1080 at 65fps, RAW10
o 1920x800 at 80fps, RAW10
o 1920x1080 at 100fps, RAW8
o 1920x1080 at 130fps, RAW8
0.1
2022/08/17
* Base driver.




# Driver controls
Gain
Control
Description
Minimum Value
Maximum Value
Default Value
Analog Gain
from x1 to x with 16 possible values
0
15
0
Digital Gain
The value is between x1 and x4096
1
4096
256
Note: Although the gains (analog and digital) are handled separately. The total gain can be seen as both gains multiplication.
Exposure
Exposure has an in between limit that if passed, will start reducing the FPS of the capture.
The exposure time is in ?s due to a conversion factor that the Jetson camera subsystem uses. So, the exposure values to be computed must be represented in 106ÿvalues as shown in the following table:
Resolution
Minimum Value
Maximum Value without reducing FPS*
Maximum Value reducing FPS
Default Value
1920x1080 RAW10
1
15339
200000
14040
1920x800 RAW10
1
12453
200000
10400
1920x1080 RAW8
1
9970
200000
9180
1920x800 RAW8
1
7666
200000
6800
*Note: After this value, the sensor also raises a flag in registerÿ0x54


Frame rate
The Frame rate values to be computed must be represented in 106ÿvalues.
Resolution
Minimum Value
Maximum Value
Default Value
1920x1080 RAW10
5000000
65000000
65000000
1920x800 RAW10
5000000
80000000
80000000
1920x1080 RAW8
5000000
100000000
100000000
1920x800 RAW8
5000000
130000000
130000000
Test pattern
Pattern mode
Description
0
Test pattern disabled
1
Front pattern: applied at the processing input
2
Rear pattern: applied at the MIPI output
Flip
Value Effect
Description
0
Not even vertical or horizontal flip
1
Vertical flip
2
Horizontal flip
3
Vertical and horizontal flip



# Update current build
Important: This step is only needed if a previous environment has been set up. If not, please continue with the Build from scratch section.
Move to the Linux for tegra directory

```cd ~/JetPack-4.6.2/JetPack_4.6.2_Linux_JETSON_NANO_TARGETS/Linux_for_Tegra/sources/```

## Apply the new driver patch
1. Undo the last driver patch:
Important: This step is only needed if a previous environment has an added driver. If not, please continue with step 2
quilt pop -a

rm -rf patches/

2. Please extract the contents provided inÿ4.6.2_evm_topaz2m_vX.Y.Z.tarÿ (with X.Y.Z the driver version).inÿsourcesÿdirectory: 
tar -xf 4.6.2_evm_topaz2m_vX.Y.Z.tar
Now, to verify the existence of all the patch files, please run the following commands on a command line:
echo -n "Patches directory check: ";if test -e "patches"; then echo "PASS"; else echo "FAIL"; fi;
echo -n "Series file check: ";if test -f "patches/series"; then echo "PASS"; else echo "FAIL"; fi;
while read p; do echo -n "$p file check: "; if test -f "patches/$p"; then echo "PASS"; else echo "FAIL";fi; done < patches/series
You can then apply the patch:
quilt push -a
After applying the patch, return to the Linux_for_Tegra directory.
cd ..

# Build from scratch
Download JetPack
IMPORTANT: Starting with a fresh JetPack installation is needed
This step has to be done on a host PC running with Ubuntu 18.04
1. Download and install the SDK manager: NVIDIA SDK Manager | NVIDIA Developer
2. Create the needed directory for the desired JetPack version:
mkdir -p ~/JetPack-4.6.2/downloads
3. Open SDKManager. Select the right hardware and JetPack version:

4. Create directories for installation:

* Download folder:ÿ~/JetPack-4.6.2/downloads
* Target HW image folder:ÿ~/JetPack-4.6.2
4.ÿSkip the flashing and installing and click on finish.


Get the sources
1.ÿMove to the Linux_for_Tegra directory:
cd ~/JetPack-4.6.2/JetPack_4.6.2_Linux_JETSON_NANO_TARGETS/Linux_for_Tegra/
2.ÿThere you will find the source_sync.sh script. Please run the following command to get Jetpack 4.6 sources:
./source_sync.sh -t tegra-l4t-r32.7.1
Apply the driver patch
1. Move to the Linux for tegra directory
cd ~/JetPack-4.6.2/JetPack_4.6.2_Linux_JETSON_NANO_TARGETS/Linux_for_Tegra/sources/
2. Please extract the contents provided inÿ4.6.2_evm_topaz2m_vX.Y.Z.tarÿ (with X.Y.Z the driver version).inÿsourcesÿdirectory:
tar -xf 4.6.2_evm_topaz2m_vX.Y.Z.tar
Now, to verify the existence of all the patch files, please run the following commands on a command line:
echo -n "Patches directory check: ";if test -e "patches"; then echo "PASS"; else echo "FAIL"; fi;
echo -n "Series file check: ";if test -f "patches/series"; then echo "PASS"; else echo "FAIL"; fi;
while read p; do echo -n "$p file check: "; if test -f "patches/$p"; then echo "PASS"; else echo "FAIL";fi; done < patches/series
You can then apply the patch:
quilt push -a
After applying the patch, return to the Linux_for_Tegra directory.
cd ..



Compile the driver
Setup the toolchain
1.ÿCreate the necessary directories for the toolchain:
mkdir -p ~/JetPack-4.6.2/toolchain
cd ~/JetPack-4.6.2/toolchain/
2.ÿGet the toolchain tarball:
wget http://releases.linaro.org/components/toolchain/binaries/7.3-2018.05/aarch64-linux-gnu/gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu.tar.xz
3.ÿExtract the contents and remove the toolchain tarball:
tar xvf gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu.tar.xz
rm  gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu.tar.xz
Compiling setup
1.ÿDefine environment variables:
# Variable to the Linux for Tegra directory
export DEVDIR=~/JetPack-4.6.2/JetPack_4.6.2_Linux_JETSON_NANO_TARGETS/Linux_for_Tegra/
2.ÿCreate the necessary directories:
cd $DEVDIR
# Create the directory to store the compiled image and dtb
mkdir -p $DEVDIR/images/dtb
3.ÿDefine the environment variables:
export TEGRA_KERNEL_OUT=$DEVDIR/images
export ARCH=arm64
export KERNEL_DIR=$DEVDIR/sources/kernel/kernel-4.9
export CROSS_COMPILE=~/JetPack-4.6.2/toolchain/gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-
export LOCALVERSION=-tegra
4.ÿClean the environment:
cd $KERNEL_DIR
make mrproper
Compile
1.ÿConfigure the kernel:
cd $KERNEL_DIR
make O=$TEGRA_KERNEL_OUT tegra_defconfig
make O=$TEGRA_KERNEL_OUT menuconfig
In the terminal menu that appears, select:
Note:ÿBy default, the driver is selected with an asterisk. For that reason, if you go back by hitting the doubleÿEscÿkey the message:ÿDo you want to save your new configuration?ÿwill not appear.
Device Drivers  --->
  <*> Multimedia support  --->
      NVIDIA overlay Encoders, decoders, sensors and other helper chips  --->
          <*> TOPAZ2M camera sensor support
If the driver is not selected, press theÿYÿkey in order to select the TOPAZ2M option. Go back by hitting the doubleÿEscÿkey until you get the message:ÿDo you want to save your new configuration?, selectÿYesÿand pressÿEnter'
Note: if the ?menuconfig? command doesn?t work, please run the following command:
sudo apt-get install libncurses5-dev libncursesw5-dev
2.ÿCompile the kernel:
make O=$TEGRA_KERNEL_OUT CROSS_COMPILE=${CROSS_COMPILE} -j4 zImage
3.ÿCompile the device tree:
make O=$TEGRA_KERNEL_OUT CROSS_COMPILE=${CROSS_COMPILE} -j4 dtbs


Install the driver
Putting the board in Force Recovery Mode (FCM)
To set the device to Force Recovery Mode, the FC REC and GND pins must be connected before powering up the board.
For the Jetson Nano Development Kit B01 model, the connection is done like this:


Next, plug the Jetson Nano board into the computer using a microUSB cable. The Force Recovery Mode can be verified by running theÿlsusbÿcommand in a terminal on the computer. The terminal should print a line similar to the following:
Bus 001 Device 015: ID 0955:7f21 NVidia Corp.


Prepare to flash
In order to install the driver, the jetson nano devkit board needs to be flashed. To accomplish this, all the compiled files need to be moved to the right directories.
1.ÿCopy the compiled image to the kernel directory.
cp $TEGRA_KERNEL_OUT/arch/arm64/boot/Image $TEGRA_KERNEL_OUT/arch/arm64/boot/zImage $DEVDIR/kernel/
2.ÿCopy the compiled device tree to the kernel directory.
cp -r $TEGRA_KERNEL_OUT/arch/arm64/boot/dts/* $DEVDIR/kernel/dtb/

Then 3 different methods are proposed to flash the board:
* Normal flash
* Massflash
* Only copy the kernel and device tree

Normal flash
1.ÿConnect the carrier board to the computer through a microUSB cable and set the jetson nano devkit board toÿForce Recoveryÿmode.
2.ÿFlash the board:
cd $DEVDIR
sudo ./flash.sh jetson-nano-qspi-sd mmcblk0p1
3.ÿReboot the board after the flashing is completed.


Massflash
Massflash is NVIDIA's tool for flashing multiple Jetson boards simultaneously when on production. It provides a way to generate images in trusted environments. The massflash blobs are portable, encrypted, and provide signed binary firmware and tool files. It is the recommended method for flashing devices when ready for production.
In case a massflash blob has been provided by your support team, go directly to the step 2.
1. To generate the massflash blob, please run the following command from the Linux_for_Tegra directory:
cd ~/JetPack-4.6.2/JetPack_4.6.2_Linux_JETSON_NANO_TARGETS/Linux_for_Tegra/
sudo BOARDID=3448 BOARDSKU=0000 FAB=400 FUSELEVEL=fuselevel_production ./nvmassflashgen.sh  jetson-nano-devkit mmcblk0p1
It is recommended to create a separate directory to perform the massflash process because the massflash is a self-contained environment.
2. Create the directory for the massflash files:
mkdir -p ~/teledyne_topaz2m_massflash
cd teledyne_topaz2m_massflash
3. Copy the massflash blob file mfi_jetson-nano-devkit.tbz2 in this directory and extract it:
tar -xvjf mfi_jetson-nano-devkit.tbz2
4. Enter the extracted directory:
cd mfi_jetson-nano-devkit
5. Make sure the board is in recovery mode and plugged to your computer.
6. Start flashing the Jetson board:
sudo ./nvmflash.sh --showlogs
7. Another terminal will be opened on your computer. When the flash finished, a message like the following will be shown:
*** Writing the BCT succeeded.
*** Rebooting the device
/home/teledyne/teledyne_topaz2m_massflash/mfi_jetson-nano-devkit/tegradevflash --instance 1-1 --reboot coldboot
Cboot version 00.01.0000
*** Rebooting the device succeeded.


Also, in the original terminal, a message like the following will be shown:
Start flashing device: 1-1, PID: 1716
Flash complete (SUCCESS)
Only copy the kernel and device tree
Note: This method might cause problems for the board to load the custom device tree. If you happen to have an already flashed and configured Jetson Nano with Jetpack 4.6.2, you could follow these steps to get the driver running.
1. Copy the compiled kernel and device trees to the board. From theÿLinux_for_Tegraÿdirectory, run:
scp $TEGRA_KERNEL_OUT/arch/arm64/boot/Image <user>@<ip_address>:/tmp
scp $TEGRA_KERNEL_OUT/arch/arm64/boot/dts/tegra210-p3448-0000-p3449-0000-b00.dtb <user>@<ip_address>:/tmp
Where theÿ<user>ÿis the Jetson Nano username and theÿ<ip_address>ÿis the board IP address.
2. Then, from the board, move the kernel and device tree files toÿ/boot:
sudo mv /tmp/Image /boot
sudo mv /tmp/tegra210-p3448-0000-p3449-0000-b00.dtb /boot
3. Set the board to load your device tree by modifyingÿ/boot/extlinux/extlinux.confÿfile, includingÿFDT /boot/tegra210-p3448-0000-p3449-0000-b00.dtbÿunderÿLABEL primary:
LABEL primary
MENU LABEL primary kernel
LINUX /boot/Image
FDT /boot/tegra210-p3448-0000-p3449-0000-b00.dtb
INITRD /boot/initrd
APPEND ${cbootargs} quiet root=/dev/mmcblk0p1 rw rootwait rootfstype=ext4 console=ttyS0,115200n8 console=tty0 fbcon=map:0 net.ifnames=0


Driver testing
Basic driver testing
This driver supports two cameras connected to the CSI-AB ports. It is set up as follows:
* camera0: This is the camera connected to the CSI-AB port, its device path isÿ/dev/video0.
* camera1: This is the camera connected to the CSI-AB port, its device path isÿ/dev/video1
Verify the driver was loaded correctly:
dmesg | grep topaz2m
* The output should look something like the following:
teledyne@teledyne-desktop:~$ dmesg | grep topaz2m
[    1.288275] topaz2m 7-0010: tegracam sensor driver:topaz2m_v2.0.6
[    1.311786] topaz2m 7-0010: TOPAZ2M sensor found
[    1.315303] topaz2m 7-0010: detected topaz2m sensor
[    1.506193] vi 54080000.vi: subdev topaz2m 7-0010 bound
* If two cameras are connected, the output will be:
teledyne@teledyne-desktop:~$ dmesg | grep topaz2m
[    1.288275] topaz2m 7-0010: tegracam sensor driver:topaz2m_v2.0.6
[    1.311786] topaz2m 7-0010: TOPAZ2M sensor found
[    1.315303] topaz2m 7-0010: detected topaz2m sensor
[    1.315998] topaz2m 8-0010: tegracam sensor driver:topaz2m_v2.0.6
[    1.339451] topaz2m 8-0010: TOPAZ2M sensor found
[    1.343063] topaz2m 8-0010: detected topaz2m sensor
[    1.506193] vi 54080000.vi: subdev topaz2m 7-0010 bound
[    1.506928] vi 54080000.vi: subdev topaz2m 8-0010 bound
Verify the devices are created:
ls /dev/video*
* The output should be the following:
teledyne@teledyne-desktop:~$ ls /dev/video*
/dev/video0
* If two cameras are connected, the output will be:
teledyne@teledyne-desktop:~$ ls /dev/video*
/dev/video0
/dev/video1
Optional
These steps are useful if the board was already flashed and confirmation that the new image and dtb are correctly installed is needed.
* Confirm the kernel installation:
uname -a
The output should state the date and time the image was compiled. For example:
Linux teledyne-desktop 4.9.253-tegra #1 SMP PREEMPT Tue Aug 16 14:33:58 CST 2022 aarch64 aarch64 aarch64 GNU/Linux
* Confirm the DTB installation
dmesg | grep dts
The output should state the dts file name corresponding with the compiled DTB. For example:
teledyne@teledyne-desktop:~$ dmesg | grep dts
[    0.211619] DTS File Name: /home/esalazar/JetPack-4.6.2/JetPack_4.6.2_Linux_JETSON_NANO_TARGETS/Linux_for_Tegra/sources/kernel/kernel-4.9/arch/arm64/boot/dts/../../../../../../hardware/nvidia/platform/t210/porg/kernel-dts/<dtb>
[    0.416273] DTS File Name: /home/esalazar/JetPack-4.6.2/JetPack_4.6.2_Linux_JETSON_NANO_TARGETS/Linux_for_Tegra/sources/kernel/kernel-4.9/arch/arm64/boot/dts/../../../../../../hardware/nvidia/platform/t210/porg/kernel-dts/<dtb>
Whereÿdtbÿcould be:
tegra210-p3448-0000-p3449-0000-b00.dtb
tegra210-p3448-0000-p3449-0000-a02.dts
Since this delivery supports the driver sensor for both A02 and BO1 Jetson Nano boards.
Important:ÿIn the DTB messages will be printed the host username computer where the DTB was compiled. If you compile the DTB and flash the Jetson Nano board, your username should be printed.
* Also, the DTB buildtime can confirm as follows:
cat /proc/device-tree/nvidia,dtbbuildtime
The output should state the date and time the DTB was compiled. For example:
teledyne@teledyne-desktop:~$ cat /proc/device-tree/nvidia,dtbbuildtime
Oct  3 202211:11:07
Capture with v4l2-ctl
In the following instructions, examples will be provided usingÿ/dev/video0. You can run these instructions for the second sensor by replacingÿ/dev/video0ÿforÿ/dev/video1ÿin all the commands.
* Install v4l utils:
sudo apt install v4l-utils
* Test the capture framerate:
1920x1080 at 65 fps, RAW10
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='Y10 ' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=0 --stream-mmap
The output should look like the following:
teledyne@teledyne-desktop:~$ v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='Y10 ' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=0 --stream-mmap
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 65.00 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 65.00 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 65.11 fps
* Capture a single frame:
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='Y10 ' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=0 --stream-mmap --stream-count=1 --stream-to=test_frame_65fps.raw
This captured frame can be viewed withÿrawpixelsÿorÿvooya. Please consider the following settings to be able to view it correctly:
* rawpixels:
-ÿwidth:ÿ1920
-ÿheight:ÿ1080
-ÿPredefined format:ÿGrayscale 8bit
-ÿPixel format:ÿGrayscale
-ÿbpp1:ÿ16
- Little Endian box checked
Note:ÿTo capture in the Jetson Nano Board the Y10 is needed for capturing in Grayscale format. This format comes in 2 bytes with a padding of zeros for the remaining bits. Since rawpixels requires the bit depth specification for Y10 to be 16bpps, the image looks darker. To see the image as found attached, unchecked the Little Endian box option.
* vooya:
-ÿwidth:ÿ1920
-ÿheight:ÿ1080
-ÿFrames/Second:ÿ65
-ÿColor Space:ÿSingle Channel
-ÿData Container:ÿSingle Integer
-ÿBit Depth (Value):ÿ14bit
Note1:ÿBit depth needs to be 14 due to how the Jetson boards pack pixels. The image looks darker.
Note2: To see the frame as has been shown before, the bit depth needs to be 10.
1920x800 at 80 fps, RAW10
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='Y10 ' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=1 --stream-mmap
teledyne@teledyne-desktop:~$ v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='Y10 ' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=1 --stream-mmap
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 80.19 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 80.09 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 80.06 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 80.04 fps
* Capture a single frame:
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='Y10 ' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=1 --stream-mmap --stream-count=1 --stream-to=test_frame_80fps.raw
This captured frame can be viewed with rawpixels or vooya. Please consider the following settings to be able to view it correctly taking into account the setting mentioned in the previous mode:
* rawpixels:ÿOnly change the height variable and Frame/Second
-ÿheight:ÿ800
* vooya:ÿOnly change the height variable and Frame/Second
-ÿheight:ÿ800
-ÿFrames/Second:ÿ80
-ÿBit Depth (Value):ÿ14bit or 10bit
1920x1080 at 100 fps, RAW8
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=2 --stream-mmap
The output should look like the following:
teledyne@teledyne-desktop:~$ v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat=GREY --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=2 --stream-mmap
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 101.00 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 100.50 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 100.33 fps
* Capture a single frame:
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat=GREY --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=2 --stream-mmap --stream-count=1 --stream-to=test_frame_100fps.raw
This captured frame can be viewed with rawpixels or vooya. Please consider the following settings to be able to view it correctly:
* rawpixels:
-ÿwidth:ÿ1920
-ÿheight:ÿ1080
-ÿPredefined format:ÿGrayscale 8bit
-ÿPixel format:ÿGrayscale
-ÿbpp1:ÿ8
- Little Endian box checked
* vooya:
-ÿwidth:ÿ1920
-ÿheight:ÿ1080
-ÿFrames/Second:ÿ100
-ÿColor Space:ÿSingle Channel
-ÿData Container:ÿSingle Integer
-ÿBit Depth (Value):ÿ8bit


1920x800 at 130 fps, RAW8
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='GREY' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=3 --stream-mmap
teledyne@teledyne-desktop:~$ v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='GREY' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=3 --stream-mmap
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 130.00 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 130.00 fps
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 130.00 fps
* Capture a single frame:
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='GREY' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=3 --stream-mmap --stream-count=1 --stream-to=test_frame_130fps.raw
This captured frame can be viewed with rawpixels or vooya. Please consider the following settings to be able to view it correctly taking into account the setting mentioned in the previous mode:
* rawpixels:ÿOnly change the height variable and Frame/Second
-ÿheight:ÿ800
* vooya:ÿOnly change the height variable and Frame/Second
-ÿheight:ÿ800
-ÿFrames/Second:ÿ130


Capture with GStreamer
Important:ÿY10 is not supported by v4l2src. For that reason, the following pipeline is for testing the RAW8 configuration.
Important:ÿTo achieve the maximum performance at 130 fps on the Jetson Nano Devkit board, you need to run the following command on the terminal everytime the board is power on:
sudo jetson_clocks
1920x1080 at 100 fps, RAW8
1. Before executing the pipeline, it is necessary to set the 1920x1080 at 100fps, RAW8 mode.
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
2. GStreamer pipeline using software elements:
gst-launch-1.0 v4l2src ! "video/x-raw,width=1920,height=1080,format=GRAY8" ! queue ! videoconvert ! queue ! xvimagesink sync=false
3. GStreamer pipeline using accelerated elements:
gst-launch-1.0 v4l2src ! "video/x-raw,width=1920,height=1080,format=GRAY8" ! nvvidconv ! 'video/x-raw(memory:NVMM),format=I420' ! nvoverlaysink sync=0
When a second camera is connected to the carrier board, the video stream can be started in parallel, from a second terminal:
v4l2-ctl -d /dev/video1 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
gst-launch-1.0 v4l2src device=/dev/video1 ! "video/x-raw,width=1920,height=1080,format=GRAY8" ! queue ! videoconvert ! queue ! xvimagesink sync=false



1920x800 at 130 fps, RAW8
1. Before executing the pipeline, it is necessary to set the 1920x800 at 130fps, RAW8 mode.
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='GREY' --set-ctrl sensor_mode=3
2. GStreamer pipeline using software elements:
gst-launch-1.0 v4l2src ! "video/x-raw,width=1920,height=800,format=GRAY8" ! queue ! videoconvert ! queue ! xvimagesink sync=false
3. GStreamer pipeline using accelerated elements:
gst-launch-1.0 v4l2src ! "video/x-raw,width=1920,height=800,format=GRAY8" ! nvvidconv ! 'video/x-raw(memory:NVMM),format=I420' ! nvoverlaysink sync=0
When a second camera is connected to the carrier board, the video stream can be started in parallel, from a second terminal:
v4l2-ctl -d /dev/video1 --set-fmt-video=width=1920,height=800,pixelformat='GREY' --set-ctrl sensor_mode=3
gst-launch-1.0 v4l2src device=/dev/video1 ! "video/x-raw,width=1920,height=800,format=GRAY8" ! queue ! videoconvert ! queue ! xvimagesink sync=false



Verify framerate
* First, in the board, download the necessary gstreamer dependencies:
sudo apt update
sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
* Clone the perf GStreamer element on the Jetson Nano Devkit board:
cd
git clone https://github.com/RidgeRun/gst-perf
cd gst-perf
* Compile and install the gst-perf element:
./autogen.sh
./configure --prefix /usr/ --libdir /usr/lib/aarch64-linux-gnu/
make
sudo make install
* Test the framerate for 100fps configuration with the help of the gst-perf element:
Before executing the pipeline, it is necessary to set the 1920x1080 at 100fps, RAW8 mode.
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
gst-launch-1.0 v4l2src ! perf ! "video/x-raw,width=1920,height=1080,format=GRAY8" ! fakesink
* The output should look like the following:
perf: perf0; timestamp: 1:04:35.561164373; bps: 1658880000,000; mean_bps: 1658880000,000; fps: 99,995; mean_fps: 99,957
INFO:
perf: perf0; timestamp: 1:04:36.570362417; bps: 1658880000,000; mean_bps: 1658880000,000; fps: 100,079; mean_fps: 99,998
INFO:
perf: perf0; timestamp: 1:04:37.579954997; bps: 1675468800,000; mean_bps: 1664409600,000; fps: 100,040; mean_fps: 100,008
INFO:
perf: perf0; timestamp: 1:04:38.589553097; bps: 1658880000,000; mean_bps: 1663027200,000; fps: 100,040; mean_fps: 100,015
* Test the framerate for 130fps configuration with the help of the gst-perf element:
Before executing the pipeline, it is necessary to set the 1920x800 at 130fps, RAW8 mode.
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='GREY' --set-ctrl sensor_mode=3
gst-launch-1.0 v4l2src ! perf ! "video/x-raw,width=1920,height=800,format=GRAY8" ! fakesink
* The output should look like the following:
perf: perf0; timestamp: 1:07:03.794964686; bps: 1400832000,000; mean_bps: 0,000; fps: 130,012; mean_fps: 130,012
INFO:
perf: perf0; timestamp: 1:07:04.794993270; bps: 1597440000,000; mean_bps: 1597440000,000; fps: 129,996; mean_fps: 130,004
INFO:
perf: perf0; timestamp: 1:07:05.795015708; bps: 1597440000,000; mean_bps: 1597440000,000; fps: 129,997; mean_fps: 130,002
INFO:
perf: perf0; timestamp: 1:07:06.795042468; bps: 1597440000,000; mean_bps: 1597440000,000; fps: 129,997; mean_fps: 130,001


Controls testing and commands
In the following instructions, examples will be provided usingÿ/dev/video0. You can run these instructions for the second sensor by replacingÿ/dev/video0ÿforÿ/dev/video1ÿin all the commands.
1. The camera must be capturing to use the controls. Since the patch for RAW10 support on GStreamer is not applied, it is better to test the controls with the RAW8 GStreamer pipeline resolutions.
* 1920x1080 at 100 fps, RAW8
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
gst-launch-1.0 v4l2src ! "video/x-raw,width=1920,height=1080,format=GRAY8" ! queue ! videoconvert ! queue ! xvimagesink sync=false
* 1920x800 at 130 fps, RAW8
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=800,pixelformat='GREY' --set-ctrl sensor_mode=3
gst-launch-1.0 v4l2src ! "video/x-raw,width=1920,height=800,format=GRAY8" ! queue ! videoconvert ! queue ! xvimagesink sync=false
2. Open up a different terminal, while GStreamer keeps displaying the captured data.
3. Start modifying the driver control values using V4L2-CTL and you should see changes on the displayed image:
Analog gain
v4l2-ctl --device=/dev/video0 -c analog_gain=<value>
The Analog gain goes fromÿx1ÿtoÿx16ÿwith 16 possible values. That is why the user can only be able to replace theÿ<value>ÿparameter with an integer number between the range mentioned.


Digital gain
v4l2-ctl --device=/dev/video0 -c digital_gain=<value>
The Digital is calculated by the sensor using the following equation:
Digital_gain = Register_value_in_12_bits / 256
Where:
* Register_value_in_12_bits is theÿ<value>ÿenter by the user
That is why the user can only be able to replace theÿ<value>ÿparameter with an integer number from 0 to 4096 (212). Since internally, the digital gain goes from x0.004 to x16.
Exposure
v4l2-ctl --device=/dev/video0 -c exposure=<value>
The exposure time is in ?s due to a conversion factor that the Jetson camera subsystem uses. So, the exposure values to be computed must be represented in 106.
For example, if aÿ<value>ÿofÿ32.8 msÿwould like to be computed, this value has to be entered by the user asÿ32800 ?s.
Frame rate
v4l2-ctl --device=/dev/video0 -c frame_rate=<value>
The frame rate values to be computed must be represented in 106ÿdue to a conversion factor that the Jetson camera subsystem uses.
For example, if aÿ<value>ÿofÿ60 fpsÿwould like to be computed, this value has to be entered by the user asÿ60000000.
Test pattern
v4l2-ctl --device=/dev/video0 -c test_pattern=<value>
Flip
v4l2-ctl --device=/dev/video0 -c flip=<value>


Testing scripts
The following scripts will help you test the different driver controls.
Analog gain testing script
./analog_gain_test.sh
#!/bin/bash

TIME_OUT=12s
ANALOG_GAIN_VALUES="10 12 14 16"

echo "Testing analog gain control"
echo "-------------------------------------"
timeout $TIME_OUT
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
timeout $TIME_OUT 
gst-launch-1.0 v4l2srcÿ! "video/x-raw,width=1920,height=1080,format=GRAY8"ÿ! textoverlay text="Analog gain control test" valignment=top halignment=center font-desc="Sans, 30"ÿ! queueÿ! videoconvertÿ! queueÿ! xvimagesink sync=false &
sleep 2;
for gain_val in $ANALOG_GAIN_VALUES
do
  v4l2-ctl --device=/dev/video0 -c analog_gain=$gain_val
  echo "Testing analog gain of $gain_val."
  sleep 2
done
pkill -9 gst-launch-1.0
echo "-------------------------------------"
echo ""
echo ""


Digital gain testing script
./digital_gain_test.sh
#!/bin/bash

TIME_OUT=12s
DIGITAL_GAIN_VALUES="384 512 1024 4096"

echo "Testing digital gain control"
echo "-------------------------------------"
timeout $TIME_OUT
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
timeout $TIME_OUT
gst-launch-1.0 v4l2srcÿ! "video/x-raw,width=1920,height=1080,format=GRAY8"ÿ! textoverlay text="Digital gain control test" valignment=top halignment=center font-desc="Sans, 30"ÿ! queueÿ! videoconvertÿ! queueÿ! xvimagesink sync=false &
sleep 2;
for gain_val in $DIGITAL_GAIN_VALUES
do
  v4l2-ctl --device=/dev/video0 -c digital_gain=$gain_val
  echo "Testing digital gain of $gain_val."
  sleep 2
done
pkill -9 gst-launch-1.0
echo "-------------------------------------"
echo ""
echo ""


Exposure testing script
./exposure_test.sh
#!/bin/bash

TIME_OUT=12s
EXPOSURE_VALUES="25000 30000 40000 60000"

echo "Testing exposure control"
echo "-------------------------------------"
timeout $TIME_OUT
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
timeout $TIME_OUT
gst-launch-1.0 v4l2srcÿ! "video/x-raw,width=1920,height=1080,format=GRAY8"ÿ! textoverlay text="Exposure control test" valignment=top halignment=center font-desc="Sans, 30"ÿ! queueÿ! videoconvertÿ! queueÿ! xvimagesink sync=false &
sleep 2;
for exp_val in $EXPOSURE_VALUES
do
  v4l2-ctl -c exposure=$exp_val
  echo "Testing exposure of $exp_val."
  sleep 2
done
pkill -9 gst-launch-1.0
echo "-------------------------------------"
echo ""


Frame rate testing script
./frame_rate_test.sh
#!/bin/bash

TIME_OUT=12s
FRAME_RATE_VALUES="90000000 55000000 43000000 28000000"

echo "Testing frame rate control"
echo "-------------------------------------"
timeout $TIME_OUT
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
timeout $TIME_OUT
gst-launch-1.0 v4l2srcÿ! perfÿ! "video/x-raw,width=1920,height=1080,format=GRAY8"ÿ! fakesink sync=false &
sleep 10;
for frame_val in $FRAME_RATE_VALUES
do
  v4l2-ctl --device=/dev/video0 -c frame_rate=$frame_val
  echo "Testing frame rate of $frame_val."
  sleep 10
done
pkill -9 gst-launch-1.0
echo "-------------------------------------"
echo ""


Test pattern testing script
./test_pattern_test.sh
#!/bin/bash

TIME_OUT=12s
TEST_PATTERN_VALUES="1 2 0"

echo "Testing test pattern control"
echo "-------------------------------------"
timeout $TIME_OUT
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
timeout $TIME_OUT
gst-launch-1.0 v4l2srcÿ! "video/x-raw,width=1920,height=1080,format=GRAY8"ÿ! textoverlay text="Test pattern control test" valignment=top halignment=center font-desc="Sans, 30"ÿ! queueÿ! videoconvertÿ! queueÿ! xvimagesink sync=false &
sleep 2;
for test_pattern_val in $TEST_PATTERN_VALUES
do
  v4l2-ctl --device=/dev/video0 -c test_pattern=$test_pattern_val
  echo "Testing test pattern of $test_pattern_val."
  sleep 3
done
pkill -9 gst-launch-1.0
echo "-------------------------------------"
echo ""


Flip testing script
./flip_test.sh
#!/bin/bash

TIME_OUT=12s
FLIP_VALUES="1 2 3 0"

echo "Testing flip control"
echo "-------------------------------------"
timeout $TIME_OUT
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='GREY' --set-ctrl sensor_mode=2
timeout $TIME_OUT
gst-launch-1.0 v4l2srcÿ! "video/x-raw,width=1920,height=1080,format=GRAY8"ÿ! textoverlay text="Flip control test" valignment=top halignment=center font-desc="Sans, 30"ÿ! queueÿ! videoconvertÿ! queueÿ! xvimagesink sync=false &
sleep 4;
for flip_val in $FLIP_VALUES
do
  v4l2-ctl --device=/dev/video0 -c flip=$flip_val
  echo "Testing flip of $flip_val."
  sleep 3
done
pkill -9 gst-launch-1.0
echo "-------------------------------------"
echo ""



Reading I2C bus address registers
For reading, and also writing, I2C bus address registers the sensor has to be capturing video usingÿv4l2-ctlÿor GStreamer. Since the addresses to be read are 8 bits and need to give back a value of 16 bits, theÿi2ctransferÿcommand can be used to read these addresses as follows in this specific case:
i2ctransfer -f -y [BUS NUMBER] w[BYTES]@[ADDRESS] [REGISTER] r[BYTES]
For example, to read the registerÿ0x7Fÿ(chip ID) of theÿ0x10ÿaddress in i2c 6 bus, it is needed to write 1 byte (0x7F) to theÿ0x10ÿaddress and read 2 bytes (r[BYTES]) from this register. So, the full command is:
i2ctransfer -f -y 6 w1@0x10 0x7F r2
The register gives back:
i2ctransfer -f -y 6 w1@0x10 0x7F r2
0x80 0x36
Alternatively, other components available on the module are always available and can be checked even if the video stream is off. For example, the EEMPROM memory can be read:

i2ctransfer -f -y 6 w2@0x50 0x06 0x90 r2
0x00 0x00 #if no-lens module connected
0x10 0x00 #if fixed-focus module connected
0x20 0x00 #if multi-focus module connected

When two modules are connected at the same time (B01 board only), the following bus ID has to be used:
* CAM0: bus 7
* CAM1: bus 8

Exemple for CAM0:
i2ctransfer -f -y 7 w2@0x50 0x06 0x90 r2
Exemple for CAM1:
i2ctransfer -f -y 8 w2@0x50 0x06 0x90 r2

Note that the bus 6 is used by the driver to communicate with the selected camera by controlling an I2C bus multiplexor. So, it can be connected to CAM0 or CAM1 depending on the last V4L2 command.




Writing I2C bus address
For reading, and also writing, I2C bus address registers the sensor has to be capturing video usingÿv4l2-ctlÿor GStreamer. For writing to an I2C bus address registers the sensor has to be capturing video usingÿv4l2-ctlÿor GStreamer. The syntaxis is similar to reading the bus registers:
i2ctransfer -f -y [BUS NUMBER] w[BYTES]@[ADDRESS] [REGISTER] [MSB VALUE TO WRITE] [MSB VALUE TO READ]
For example, to write to the registerÿ0x0Bÿ(Integration Time) of theÿ0x10ÿaddress in i2c 6 bus, it is needed to write 3 bytes (w[BYTES]) to the registerÿ0x0Bÿof theÿ0x10ÿaddress. 1 byte is for the register to be written (0x0B) and the other 2 bytes are the value to be written in this register. So, the full command for this example is:
For example:
i2ctransfer -f -y 6 w3@0x10 0x0b 0x30 0x00
If the registerÿ0x0Bÿis read:
teledyne@ubuntu:~$ i2ctransfer -f -y 6 w1@0x10 0x0b r2
0x30 0x00
When two modules are connected at the same time (B01 board only), the following bus ID has to be used:
* CAM0: bus 7
* CAM1: bus 8

Exemple for CAM0:
i2ctransfer -f -y 7 w3@0x10 0x0b 0x30 0x00
Exemple for CAM1:
i2ctransfer -f -y 8 w3@0x10 0x0b 0x30 0x00

Note that the bus 6 is used by the driver to communicate with the selected camera by controlling an I2C bus multiplexor. So, it can be connected to CAM0 or CAM1 depending on the last V4L2 command.



Driver debugging
Debug V4L2 Capture
Enable drivers debug
To enable drivers debugging, use the following command replacing <FILE> with the name of the file.
echo file <FILE> +p > /sys/kernel/debug/dynamic_debug/control    # '+p' to enable debug
To disable debug:
echo file <FILE> -p > /sys/kernel/debug/dynamic_debug/control    # '-p' to disable debug
1. Enter into the Super User mode:
sudo su
2. Run the following commands:
echo 7 >/sys/class/video4linux/video0/dev_debug
echo 8 > /proc/sys/kernel/printk
cd /sys/kernel/debug/dynamic_debug/
echo file capture_common.c +p > control # '-p' to disable debug
echo file tegracam_ctrls.c +p > control # '-p' to disable debug
echo file vi4_fops.c +p > control # '-p' to disable debug
echo file vi2_fops.c +p > control # '-p' to disable debug
echo file tegracam_v4l2.c +p > control # '-p' to disable debug
echo file csi4_fops.c +p > control # '-p' to disable debug
echo file csi5_fops.c +p > control # '-p' to disable debug
echo file csi2_fops.c +p > control # '-p' to disable debug
echo file capture.c +p > control # '-p' to disable debug
echo file channel.c +p > control # '-p' to disable debug
echo file capture_vi_channel.c +p > control # '-p' to disable debug
echo file capture_common.c +p > control # '-p' to disable debug
echo file sensor_common.c +p > control # '-p' to disable debug
echo file camera_common.c +p > control # '-p' to disable debug
echo file csi.c +p > control # '-p' to disable debug
echo file core.c +p > control # '-p' to disable debug
cd 
echo 1 > /sys/kernel/debug/tracing/tracing_on
echo 30720 > /sys/kernel/debug/tracing/buffer_size_kb
echo 1 > /sys/kernel/debug/tracing/events/tegra_rtcpu/enable
echo 1 > /sys/kernel/debug/tracing/events/freertos/enable
echo 1 > /sys/kernel/debug/tracing/events/camera_common/enable
echo > /sys/kernel/debug/tracing/trace
exit
Note:ÿthe debug messages will be visible either throughÿdmesgÿor using UART. In the following section, you will find instructions on how to find the messages using theÿdmesgÿprocedure.


Testing the RAW10 resolution
1. Clean the kernel tracing:
sudo sh -c "echo > /sys/kernel/debug/tracing/trace"
2. The kernel tracing should print messages like the following:
teledyne@teledyne-desktop:~$ sudo cat /sys/kernel/debug/tracing/trace
# tracer: nop
#
# entries-in-buffer/entries-written: 0/0   #P:4
#
#                              _-----=> irqs-off
#                             / _----=> need-resched
#                            | / _---=> hardirq/softirq
#                            || / _--=> preempt-depth
#                            ||| /     delay
#           TASK-PID   CPU#  ||||    TIMESTAMP  FUNCTION
#              | |       |   ||||       |         |
teledyne@teledyne-desktop:~$
3. Open a second terminal in the Jetson Nano board. Clean theÿdmesgÿlog:
sudo dmesg -c
4. Wait for new kernel messages:
dmesg -wH
5. In the first terminal dequeue some buffers with the following command for the RAW10 resolution:
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat='Y10 ' --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=0 --stream-mmap --stream-count=10
6. Output messages in the second terminal related to theÿdmesgÿwill be shown similar to the following:
[  +0,001065] topaz2m 6-0010: camera_common_dpd_enable: csi 0
[  +0,000007] topaz2m 6-0010: camera_common_mclk_disable: disable MCLK
[  +0,000028] topaz2m 6-0010: camera_common_mclk_enable: enable MCLK with 50000000 Hz
[  +0,000061] topaz2m 6-0010: camera_common_dpd_disable: csi 0
[  +0,023201] video0: open (0)

[  +0,010851] video0: VIDIOC_G_FMT: type=vid-cap, width=1920, height=1080, pixelformat=Y10 , field=none, bytesperline=3840, sizeimage=4147200, colorspace=8, flags=0x0, ycbcr_enc=0, quantization=0, xfer_func=0
[  +0,018754] topaz2m 6-0010: camera_common_try_fmt: size 1920 x 1080
[  +0,006348] topaz2m 6-0010: camera_common_try_fmt: use_sensor_mode_id 1
[  +0,006733] topaz2m 6-0010: camera_common_s_fmt(8202) size 1920 x 1080
[  +0,006763] topaz2m 6-0010: camera_common_try_fmt: size 1920 x 1080
[  +0,006397] topaz2m 6-0010: camera_common_try_fmt: use_sensor_mode_id 1
[  +0,006656] video4linux video0: tegra_channel_update_format: Resolution= 1920x1080 bytesperline=3840
[  +0,009214] video0: VIDIOC_S_FMT: type=vid-cap, width=1920, height=1080, pixelformat=Y10 , field=none, bytesperline=3840, sizeimage=4147200, colorspace=8, flags=0x0, ycbcr_enc=0, quantization=0, xfer_func=0

[  +0,100810] video4linux video0: free_ring_buffers: capture init latency is 136 ms
[  +0,151988] video4linux video0: Syncpoint already enabled at capture done!0
[  +0,013576] topaz2m 6-0010: v4l2sd_stream++ enable 0
[  +0,011419] video0: VIDIOC_STREAMOFF: type=vid-cap
[  +0,017038] topaz2m 6-0010: camera_common_dpd_enable: csi 0
[  +0,005645] topaz2m 6-0010: camera_common_mclk_disable: disable MCLK
[  +0,007083] video0: release


7. Read the kernel tracing messages in the first terminal:
sudo cat /sys/kernel/debug/tracing/trace
The output should show messages similar to the following:
teledyne@teledyne-desktop:~$ sudo cat /sys/kernel/debug/tracing/trace
# tracer: nop
#
# entries-in-buffer/entries-written: 34/34   #P:4
#
#                              _-----=> irqs-off
#                             / _----=> need-resched
#                            | / _---=> hardirq/softirq
#                            || / _--=> preempt-depth
#                            ||| /     delay
#           TASK-PID   CPU#  ||||    TIMESTAMP  FUNCTION
#              | |       |   ||||       |         |
        v4l2-ctl-7936  [003] ....  1127.096116: tegra_channel_open: vi-output, topaz2m 6-0010
        v4l2-ctl-7936  [000] ....  1127.098646: camera_common_s_power: status : 0x1
        v4l2-ctl-7936  [000] ....  1127.309279: camera_common_s_power: status : 0x0
        v4l2-ctl-7936  [000] ....  1127.311396: tegra_channel_set_power: topaz2m 6-0010 : 0x1
        v4l2-ctl-7936  [000] ....  1127.311405: camera_common_s_power: status : 0x1
        v4l2-ctl-7936  [000] ....  1127.334655: tegra_channel_set_power: nvcsi--1 : 0x1
        v4l2-ctl-7936  [000] ....  1127.334663: csi_s_power: enable : 0x1
 vi-output, topa-7937  [002] ....  1127.359147: tegra_channel_set_stream: enable : 0x1
 vi-output, topa-7937  [000] ....  1127.360455: tegra_channel_set_stream: nvcsi--1 : 0x1
 vi-output, topa-7937  [000] ....  1127.360457: csi_s_stream: enable : 0x1
 vi-output, topa-7937  [000] ....  1127.360481: tegra_channel_set_stream: topaz2m 6-0010 : 0x1
 vi-output, topa-7937  [000] ....  1127.464860: tegra_channel_capture_frame: sof:1127.318815299
 vi-output, topa-7937  [000] ....  1127.480028: tegra_channel_capture_frame: sof:1127.333984882
 vi-output, topa-7937  [000] ....  1127.495288: tegra_channel_capture_frame: sof:1127.349222278
 vi-output, topa-7937  [000] ....  1127.510497: tegra_channel_capture_frame: sof:1127.364436236
 vi-output, topa-7937  [000] ....  1127.525708: tegra_channel_capture_frame: sof:1127.379648476
 vi-output, topa-7937  [000] ....  1127.541558: tegra_channel_capture_frame: sof:1127.395505976
 vi-output, topa-7937  [000] ....  1127.555994: tegra_channel_capture_frame: sof:1127.409936809
 vi-output, topa-7937  [000] ....  1127.571881: tegra_channel_capture_frame: sof:1127.425747590
 vi-output, topa-7937  [000] ....  1127.586337: tegra_channel_capture_frame: sof:1127.440279570
 vi-output, topa-7937  [000] ....  1127.601512: tegra_channel_capture_frame: sof:1127.455455872
 vi-output, topa-7937  [000] ....  1127.616692: tegra_channel_capture_frame: sof:1127.470632382
 vi-output, topa-7937  [000] ....  1127.631883: tegra_channel_capture_frame: sof:1127.485827174
 vi-output, topa-7937  [002] ....  1127.648026: tegra_channel_capture_frame: sof:1127.501981497
        v4l2-ctl-7936  [002] ....  1127.661612: tegra_channel_capture_done: mw_ack_done:1127.527535222
        v4l2-ctl-7936  [002] ....  1127.661642: tegra_channel_set_stream: enable : 0x0
        v4l2-ctl-7936  [002] ....  1127.661650: tegra_channel_set_stream: topaz2m 6-0010 : 0x0
        v4l2-ctl-7936  [000] ....  1127.662856: tegra_channel_set_stream: nvcsi--1 : 0x0
        v4l2-ctl-7936  [000] ....  1127.662876: csi_s_stream: enable : 0x0
        v4l2-ctl-7936  [001] ....  1127.670236: tegra_channel_close: vi-output, topaz2m 6-0010
        v4l2-ctl-7936  [001] ....  1127.673355: tegra_channel_set_power: topaz2m 6-0010 : 0x0
        v4l2-ctl-7936  [001] ....  1127.673365: camera_common_s_power: status : 0x0
        v4l2-ctl-7936  [001] ....  1127.675472: tegra_channel_set_power: nvcsi--1 : 0x0
        v4l2-ctl-7936  [001] ....  1127.675475: csi_s_power: enable : 0x0


Testing the RAW8 resolution
1. Repeat the same steps explained in the RAW10 resolution. However, replace the dequeue buffers command with the following:
v4l2-ctl -d /dev/video0 --set-fmt-video=width=1920,height=1080,pixelformat=GREY --set-ctrl bypass_mode=0 --set-ctrl sensor_mode=2 --stream-mmap --stream-count=10
2. Some output messages in the second terminal should change. They should read like the following:
[  +0,010893] video0: VIDIOC_G_FMT: type=vid-cap, width=1920, height=1080, pixelformat=Y10 , field=none, bytesperline=3840, sizeimage=4147200, colorspace=8, flags=0x0, ycbcr_enc=0, quantization=0, xfer_func=0
[  +0,019426] topaz2m 6-0010: camera_common_try_fmt: size 1920 x 1080
[  +0,006301] topaz2m 6-0010: camera_common_try_fmt: use_sensor_mode_id 1
[  +0,006705] topaz2m 6-0010: camera_common_s_fmt(8193) size 1920 x 1080
[  +0,006595] topaz2m 6-0010: camera_common_try_fmt: size 1920 x 1080
[  +0,006290] topaz2m 6-0010: camera_common_try_fmt: use_sensor_mode_id 1
[  +0,007627] video4linux video0: tegra_channel_update_format: Resolution= 1920x1080 bytesperline=1920
[  +0,010102] video0: VIDIOC_S_FMT: type=vid-cap, width=1920, height=1080, pixelformat=GREY, field=none, bytesperline=1920, sizeimage=2073600, colorspace=8, flags=0x0, ycbcr_enc=0, quantization=0, xfer_func=0
8. The output of the kernel tracing messages are very similar to RAW10.

Debugging using the UART on the board
Note:ÿthe information that can be seen through UART can be seen usingÿdmesg, except messages that show before booting. Therefore, for driver debuggingÿdmesgÿmost of the time is enough, however, to debug boot issues or seeing messages that appear before boot, UART is recommended.


Known issues
RAW10 format capturing
Since Y10 is not supported by v4l2src, so a patch is had to be applied to GStreamer so that v4l2src has support for capturing using the Y10 format. However, even with the patch, the buffers look very dark because GStreamer takes the full 2 bytes instead of just the 10 bits, which results in a darker image due to a 10bit image being interpreted as a 16bit image.
