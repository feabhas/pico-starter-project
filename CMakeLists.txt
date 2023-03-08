# cmake_minimum_required(VERSION 3.22)
cmake_minimum_required(VERSION 3.16)
include(/workspaces/pico-sdk/external/pico_sdk_import.cmake)
project(test_project C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

set(SRC_FILES src/test.c)

add_executable(test ${SRC_FILES})

pico_enable_stdio_usb(test 1)
pico_enable_stdio_uart(test 1) 
pico_add_extra_outputs(test) 

target_link_libraries(test pico_stdlib)