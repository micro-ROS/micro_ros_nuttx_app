![banner](.images/banner-dark-theme.png#gh-dark-mode-only)
![banner](.images/banner-light-theme.png#gh-light-mode-only)

# micro-ROS app for Nuttx RTOS

| Galactic                                                                                                                                                                                                     | Rolling                                                                                                                                                                                                 | Humble                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![Galactic](https://github.com/micro-ROS/micro_ros_nuttx_app/actions/workflows/ci.yml/badge.svg?branch=galactic&event=schedule)](https://github.com/micro-ROS/micro_ros_nuttx_app/actions/workflows/ci.yml) | [![Rolling](https://github.com/micro-ROS/micro_ros_nuttx_app/actions/workflows/ci.yml/badge.svg?branch=main&event=schedule)](https://github.com/micro-ROS/micro_ros_nuttx_app/actions/workflows/ci.yml) | [![Humble](https://github.com/micro-ROS/micro_ros_nuttx_app/actions/workflows/ci.yml/badge.svg?branch=humble&event=schedule)](https://github.com/micro-ROS/micro_ros_nuttx_app/actions/workflows/ci.yml) |


This component has been tested in Nuttx 10.1 and Nuttx 10.3.

## Dependencies

This component needs `colcon` and other Python 3 packages in order to build micro-ROS packages:

```bash
pip3 install catkin_pkg lark-parser empy colcon-common-extensions
```

## Usage

You can clone this repo directly in the `app` folder of your project.

## Example

In order to test a int32_publisher example for [STM32L4 Discovery kit IoT node](https://www.st.com/en/evaluation-tools/b-l475e-iot01a.html):

<!--
Deps:
apt install git bison flex gettext texinfo libncurses5-dev libncursesw5-dev gperf automake libtool pkg-config build-essential gperf genromfs libgmp-dev libmpc-dev libmpfr-dev libisl-dev binutils-dev libelf-dev libexpat-dev gcc-multilib g++-multilib picocom u-boot-tools util-linux kconfig-frontends gcc-arm-none-eabi binutils-arm-none-eabi python3-pip cmake sudo

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
4. Enable micro-ROS library, UART4 and Serial Termios using `make menuconfig` or:
```bash
kconfig-tweak --enable CONFIG_MICROROSLIB
kconfig-tweak --enable CONFIG_MICROROS_EXAMPLE
kconfig-tweak --enable CONFIG_SERIAL_TERMIOS
kconfig-tweak --enable CONFIG_STM32L4_UART4
kconfig-tweak --enable CONFIG_STM32L4_UART4_SERIALDRIVER
kconfig-tweak --enable CONFIG_UART4_SERIALDRIVER
kconfig-tweak --set-val CONFIG_UART4_RXBUFSIZE 256
kconfig-tweak --set-val CONFIG_UART4_TXBUFSIZE 256
kconfig-tweak --set-val CONFIG_UART4_BAUD 115200
kconfig-tweak --set-val CONFIG_UART4_BITS 8
kconfig-tweak --set-val CONFIG_UART4_PARITY 0
kconfig-tweak --set-val CONFIG_UART4_2STOP 0
```
5. Build Nuttx:
```bash
make -j$(nproc)
```
6. Flash the board:
```bash
openocd -f interface/stlink-v2-1.cfg -f target/stm32l4x.cfg -c init -c "reset halt" -c "flash write_image erase nuttx.bin 0x08000000" -c "reset" -c "exit"
```
7. Connect UART4 (the one in the Arduino header D0 and D1) to your micro-ROS Agent.
8. You can run the micro-ROS example by launching `microros /dev/ttyS1` in NSH console. First argument is the serial port used for client to agent communication:
```
NuttShell (NSH) NuttX-10.1.0-RC1
nsh> microros /dev/ttyS1
micro-ROS transport: connected using serial mode, dev: '/dev/ttyS1'
INFO: rcl_wait timeout 0 ms
Sent: 0
Sent: 1
Sent: 2
```

Is possible to use a micro-ROS Agent just with this docker command:

```bash
# Serial micro-ROS Agent
docker run -it --rm -v /dev:/dev --privileged --net=host microros/micro-ros-agent:humble serial --dev [YOUR BOARD PORT] -v6
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
