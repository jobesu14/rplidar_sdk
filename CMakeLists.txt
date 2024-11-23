cmake_minimum_required(VERSION 3.5)
project(rplidar)

# Compiler settings
set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-O2 -Wall -Wextra -Wpedantic -fPIC
    # silence the unfortunate warnings to clarifying library consumer build logs
    -Wno-unused-value
    -Wno-unused-parameter
    -Wno-pedantic
    -Wno-stringop-truncation
    -Wno-ignored-qualifiers
    -Wno-cast-function-type
    -Wno-unused-function
    -Wno-unused-result
  )
endif()

if(MSVC)
    # exit with unsupported compiler
    message(FATAL_ERROR "Unsupported compiler")
endif()

set(SLLIDAR_SDK_PATH "./sdk/")

FILE(GLOB SLLIDAR_SDK_SRC 
    "${SLLIDAR_SDK_PATH}/src/arch/linux/*.cpp"
    "${SLLIDAR_SDK_PATH}/src/hal/*.cpp"
    "${SLLIDAR_SDK_PATH}/src/*.cpp"
    "${SLLIDAR_SDK_PATH}/src/dataunpacker/*.cpp"
    "${SLLIDAR_SDK_PATH}/src/dataunpacker/unpacker/*.cpp"
)

include_directories(
    ${SLLIDAR_SDK_PATH}/include
    ${SLLIDAR_SDK_PATH}/src
)

# RP Lidar Driver library
add_library(rplidar
    ${SLLIDAR_SDK_SRC}
)

# Install targets
install(
    TARGETS
        rplidar
)

# Install headers
install(
    DIRECTORY
        sdk/include
    DESTINATION
        include/${PROJECT_NAME}
    FILES_MATCHING
        PATTERN "*.hpp"
        PATTERN "*.h"
)