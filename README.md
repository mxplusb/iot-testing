# Azure IoT Testing

To repro:

1. Make sure `vcpkg` is installed.
1. Make sure you have the cross-compilation toolkit packages and PlatformIO installed.
   1. On Ubuntu the cross-compilation packages are `g++-arm-linux-gnueabihf` and `gcc-arm-linux-gnueabihf`. You can install the PlatformIO CLI with `python3 -c "$(curl -fsSL https://raw.githubusercontent.com/platformio/platformio/master/scripts/get-platformio.py)"` ([ref](https://docs.platformio.org/en/latest//core/installation.html#super-quick-mac-linux))
1. Make sure your PlatformIO toolchain is good to go, you can run `pio check` or `pio ci -b uno .` to make sure the toolchain is good. `pio check` should be fine, but `pio ci -b uno .` will fail and that's okay.
1. Install `azure-iot-sdk-c` with `vcpkg install azure-iot-sdk-c:arm-linux` and then make it available with `vcpkg integrate install`
1. Make the build directory with `mkdir build && cd build`
1. Prep CMake with `cmake -DCMAKE_BUILD_TYPE=Debug -GNinja -DCMAKE_C_COMPILER=$HOME/.platformio/packages/toolchain-atmelavr/bin/avr-gcc -DCMAKE_CXX_COMPILER=$HOME/.platformio/packages/toolchain-atmelavr/bin/avr-g++ -DVCPKG_TARGET_TRIPLET=arm-linux -DCMAKE_TOOLCHAIN_FILE=<path_to_vcpkg_cmake> ..`
1. Build with `cmake --build .`
