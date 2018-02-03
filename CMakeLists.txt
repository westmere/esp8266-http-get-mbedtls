cmake_minimum_required(VERSION 3.5)

set(TOOLCHAIN_PREFIX "${CMAKE_CURRENT_LIST_DIR}/xtensa-lx106-elf")
set(ESP8266_SDK_BASE "${CMAKE_CURRENT_LIST_DIR}/ESP8266_RTOS_SDK")
set(ESPTOOL "${CMAKE_CURRENT_LIST_DIR}/esptool/esptool.py")
set(CMAKE_TOOLCHAIN_FILE "${CMAKE_CURRENT_LIST_DIR}/esp8266-cmake/cmake/ESP8266RTOS-1.5.0.cmake")

# cmake_policy(SET CMP0048 NEW)
project(http-get-mbedtls-demo VERSION 0.9.0 LANGUAGES C CXX ASM)

# add_definitions(-DMBEDTLS_CONFIG_FILE="${CMAKE_CURRENT_LIST_DIR}/include/mbedtls/config.h")
include_directories(include)
add_subdirectory(mbedtls-cmake)

file(WRITE "${CMAKE_BINARY_DIR}/user_config.h"
    "#ifndef __USER_CONFIG_H__\n"
    "#define __USER_CONFIG_H__\n"
    "#define SSID     \"minhha\"\n"
    "#define PASSWORD \"minhha06\"\n"
    "#endif\n"
)
add_library(http-get-mbedtls OBJECT ${PAHO_MQTT_SDK_SOURCES}
        src/mbedtls/hardware_entropy.c
        src/mbedtls/net_lwip.c
        src/http_get_mbedtls.c
        src/cert.c
)
target_include_directories(http-get-mbedtls PRIVATE     "${ESP8266_RTOS_SDK_INCLUDE}"
                                                        "${CMAKE_BINARY_DIR}"
                                                        "mbedtls-cmake/mbedtls/include"
)
# set_target_properties(http-get-mbedtls PROPERTIES COMPILE_FLAGS "-DMQTT_TASK")
esp8266_add_executable(${CMAKE_PROJECT_NAME} 
        $<TARGET_OBJECTS:http-get-mbedtls>
        $<TARGET_OBJECTS:mbedtls-object>
        $<TARGET_OBJECTS:mbedx509-object>
        $<TARGET_OBJECTS:mbedcrypto-object>
)
esp8266_add_firmware(firmware ${CMAKE_PROJECT_NAME})
