
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