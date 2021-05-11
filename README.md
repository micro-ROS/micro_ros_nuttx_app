# micro-ROS app for Nuttx RTOS

This component has been tested in Nuttx 10 and Nuttx 10.1.

## Dependencies

This component needs `colcon` and other Python 3 packages in order to build micro-ROS packages:

```bash
. $IDF_PATH/export.sh
pip3 install catkin_pkg lark-parser empy colcon-common-extensions
```

## Usage

You can clone this repo directly in the `app` folder of your project.

## Example

In order to test a int32_publisher example for [STM32L4 Discovery kit IoT node](https://www.st.com/en/evaluation-tools/b-l475e-iot01a.html):

<!-- 
Deps:
apt install git bison flex gettext texinfo libncurses5-dev libncursesw5-dev gperf automake libtool pkg-config build-essential gperf genromfs libgmp-dev libmpc-dev libmpfr-dev libisl-dev binutils-dev libelf-dev libexpat-dev gcc-multilib g++-multilib picocom u-boot-tools util-linux kconfig-frontends gcc-arm-none-eabi binutils-arm-none-eabi python3-pip cmake

pip3 install catkin_pkg lark-parser empy colcon-common-extensions
-->
1. Install all the Nuttx dependencies using the [official documentation](https://nuttx.apache.org/docs/10.0.0/quickstart/install.html)
2. Clone this repo inside `apps` folder:
```bash
git clone https://github.com/micro-ROS/micro_ros_nuttx_app apps/microros
```
3. Go to `nuttx` folder and configure the support for [STM32L4 Discovery kit IoT node](https://www.st.com/en/evaluation-tools/b-l475e-iot01a.html):
```bash
cd nuttx
./tools/configure.sh -l b-l475e-iot01a:nsh
```
4. Enable micro-ROS library and Serial Termios using `make menuconfig` or:
```bash
kconfig-tweak --enable CONFIG_MICROROSLIB
kconfig-tweak --enable CONFIG_SERIAL_TERMIOS
```
5. Build Nuttx:
```bash
make -j$(nproc)
```
5. Flash the board:
```bash
openocd -f interface/stlink-v2-1.cfg -f target/stm32l4x.cfg -c init -c "reset halt" -c "flash write_image erase nuttx.bin 0x08000000" -c "reset" -c "exit"
```

Is possible to use a micro-ROS Agent just with this docker command:

```bash
# Serial micro-ROS Agent
docker run -it --rm -v /dev:/dev --privileged --net=host microros/micro-ros-agent:main serial --dev [YOUR BOARD PORT] -v6
```
## Purpose of the Project

This software is not ready for production use. It has neither been developed nor
tested for a specific use case. However, the license conditions of the
applicable Open Source licenses allow you to adapt the software to your needs.
Before using it in a safety relevant setting, make sure that the software
fulfills your requirements and adjust it according to any applicable safety
standards, e.g., ISO 26262.

## License

This repository is open-sourced under the Apache-2.0 license. See the [LICENSE](LICENSE) file for details.

For a list of other open-source components included in ROS 2 system_modes,
see the file [3rd-party-licenses.txt](3rd-party-licenses.txt).

## Known Issues/Limitations

There are no known limitations.
