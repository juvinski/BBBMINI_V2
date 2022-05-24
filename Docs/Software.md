
Download the image 

1) https://rcn-ee.net/rootfs/bb.org/testing/2022-05-01/buster-console/bone-debian-10.12-console-armhf-2022-05-01-1gb.img.xz
2) Update software: sudo apt update && sudo apt upgrade -y
3) Install software: sudo apt install -y bb-cape-overlays cpufrequtils g++ pkg-config gawk git make screen python python-dev python-lxml python-pip python-future
4) Update scripts: cd /opt/scripts && sudo git pull
5) Expend partition: sudo /opt/scripts/tools/grow_partition.sh
6) Install RT Kernel: sudo /opt/scripts/tools/update_kernel.sh --bone-rt-kernel --lts-5_10
7) Install python dependencies
	* python -m pip install empy
	* python -m pip install pexpect

## Compile ArduPilot native on BeagleBone
1. `cd ~`
2. `git clone https://github.com/ardupilot/ardupilot.git`
3. `cd ardupilot`
4.  * for ArduCopter `git checkout Copter-4.2`
    * for ArduPlane `git checkout ArduPlane-4.2
    * for ArduRover `git checkout Rover-4.2`
    * for ArduSub `git checkout Sub-4.1`
5. `git submodule update --init --recursive`
6. `./waf configure --board=bbbmini-v2`
7. `./waf` (take about 1h20m)
8. `cp build/bbbmini/bin/* /home/debian/`

## Compiling and loading DTO for BBBMini

1. Clone and compile the dtc - device tree compiler `git clone git://git.kernel.org/pub/scm/utils/dtc/dtc.git`
2. Change the directory `cd dtc`
3. Make `make`
4. Install `make install`
5. Check the version `dtc --version`
	The return shoud be `version 1.6.1`
6. Change Directory `cd ~`
7. Clone BeagleBoard's device tree `git clone https://github.com/beagleboard/BeagleBoard-DeviceTrees.git`
8. Change the directory `cd BeagleBoard-DeviceTrees`
9. Checkout the 5.10 `git checkout v5.10.x`
10. Get the dts updated `wget https://raw.githubusercontent.com/juvinski/BBBMINI_V2/main/DeviceTreeOverlay/am335x-boneblack-bbbmini.dts -O src/arm/am335x-boneblack-bbbmini.dts`
11. Make `make`
12. Install `make install`
13. Check the new dtb file `ls /boot/dtbs/5.10.109-bone-rt-r63/am335x-boneblack-bbbmini.dtb` - should return file path and name
14. Change the uEnv file ` vi /boot/uEnv.txt`
15. Change the lines:
	Add the dtb file: `dtb=am335x-boneblack-bbbmini.dtb`
	Comment the uboot_overlay: `#enable_uboot_overlays=1`
	Uncomment the PRU overlay: `uboot_overlay_pru=AM335X-PRU-UIO-00A0.dtbo` - all other dtbo for PRU must be commented
16. Save the file `esc + :x!`
17. Reboot
18. After the boot, to ensure the dtb was loaded run a `ls /dev/spidev*` 
	The return should be `/dev/spidev* /dev/spidev0.0  /dev/spidev1.0  /dev/spidev1.1`
19. Proceed to ardupilot compilation