cmake_minimum_required(VERSION 3.13)
project(iot_blank)

set(CMAKE_CXX_FLAGS "-fno-exceptions -fno-threadsafe-statics -fpermissive -std=gnu++11 -Os -Wall -ffunction-sections -fdata-sections -flto")
set(CMAKE_C_FLAGS "-std=gnu11 -fno-fat-lto-objects -Os -Wall -ffunction-sections -fdata-sections -flto -mmcu=atmega328p")

set(CMAKE_CONFIGURATION_TYPES "uno" CACHE STRING "Build Types reflect PlatformIO Environments" FORCE)

add_definitions(-D'PLATFORMIO=50101')
add_definitions(-D'ARDUINO_AVR_UNO')
add_definitions(-D'F_CPU=16000000L')
add_definitions(-D'ARDUINO_ARCH_AVR')
add_definitions(-D'ARDUINO=10808')
add_definitions(-D'__AVR_ATmega328P__')

include_directories("${CMAKE_CURRENT_LIST_DIR}/include")
include_directories("${CMAKE_CURRENT_LIST_DIR}/src")
FILE(GLOB_RECURSE SRC_LIST
        ${CMAKE_CURRENT_LIST_DIR}/src/*.*
        ${CMAKE_CURRENT_LIST_DIR}/lib/*.*
        )
list(APPEND SRC_LIST ${EXTRA_LIB_SOURCES})

include_directories("$ENV{HOME}/.platformio/packages/framework-arduino-avr/cores/arduino")
include_directories("$ENV{HOME}/.platformio/packages/framework-arduino-avr/variants/standard")
include_directories("$ENV{HOME}/.platformio/packages/framework-arduino-avr/libraries/EEPROM/src")
include_directories("$ENV{HOME}/.platformio/packages/framework-arduino-avr/libraries/HID/src")
include_directories("$ENV{HOME}/.platformio/packages/framework-arduino-avr/libraries/SPI/src")
include_directories("$ENV{HOME}/.platformio/packages/framework-arduino-avr/libraries/SoftwareSerial/src")
include_directories("$ENV{HOME}/.platformio/packages/framework-arduino-avr/libraries/Wire/src")
include_directories("$ENV{HOME}/.platformio/packages/toolchain-atmelavr/lib/gcc/avr/7.3.0/include")
include_directories("$ENV{HOME}/.platformio/packages/toolchain-atmelavr/lib/gcc/avr/7.3.0/include-fixed")
include_directories("$ENV{HOME}/.platformio/packages/toolchain-atmelavr/avr/include")

add_custom_target(
        Production ALL
        COMMAND platformio -c clion run "$<$<NOT:$<CONFIG:All>>:-e${CMAKE_BUILD_TYPE}>"
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

add_custom_target(
        Debug ALL
        COMMAND platformio -c clion run --target debug "$<$<NOT:$<CONFIG:All>>:-e${CMAKE_BUILD_TYPE}>"
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

add_executable(Z_DUMMY_TARGET ${SRC_LIST})

find_package(azure_iot_sdks CONFIG REQUIRED)

# all of the library options...
# hsm_security_client prov_auth_client iothub_service_client iothub_client iothub_client_http_transport iothub_client_amqp_transport iothub_client_amqp_ws_transport iothub_client_mqtt_transport iothub_client_mqtt_ws_transport serializer
link_libraries(iothub_client
        iothub_message
        iothub_client_http_transport
        serializer
        azure_c_shared_utility)
