cmake_minimum_required(VERSION 3.13)
# initialize the SDK based on PICO_SDK_PATH
# note: this must happen before project()
include(pico_sdk_import.cmake)

set(PROGRAM_NAME "gbusb")
project(${PROGRAM_NAME})
 
pico_sdk_init()

add_executable(${PROGRAM_NAME})

pico_generate_pio_header(${PROGRAM_NAME} ${CMAKE_CURRENT_LIST_DIR}/pio/spi.pio)

target_include_directories(${PROGRAM_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR})

target_sources(${PROGRAM_NAME} PRIVATE
        main.c

        usb_descriptors.c

        # PIO components
        pio/pio_spi.c
        )
        
target_link_libraries(${PROGRAM_NAME} PRIVATE pico_stdlib hardware_pio tinyusb_device tinyusb_board)
pico_add_extra_outputs(${PROGRAM_NAME})

add_custom_target(cleanall
        COMMAND rm -r ${CMAKE_CURRENT_LIST_DIR}/pico-sdk
        COMMAND rm -r ${CMAKE_CURRENT_LIST_DIR}/CMakeFiles
        COMMAND rm -r ${CMAKE_CURRENT_LIST_DIR}/elf2uf2
        COMMAND rm -r ${CMAKE_CURRENT_LIST_DIR}/generated
        COMMAND rm -r ${CMAKE_CURRENT_LIST_DIR}/pioasm
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/cmake_install.cmake
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/CMakeCache.txt
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/Makefile
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/${PROGRAM_NAME}.bin
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/${PROGRAM_NAME}.dis
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/${PROGRAM_NAME}.elf
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/${PROGRAM_NAME}.elf.map
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/${PROGRAM_NAME}.hex
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/${PROGRAM_NAME}.uf2
        COMMAND rm ${CMAKE_CURRENT_LIST_DIR}/spi.pio.h)