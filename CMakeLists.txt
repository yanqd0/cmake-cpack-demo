cmake_minimum_required(VERSION 3.10.2)

set(CMAKE_BUILD_TYPE Release)
set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}" "${CMAKE_CURRENT_SOURCE_DIR}/build/cmake")

execute_process(
    COMMAND git describe --abbrev=0
    OUTPUT_VARIABLE PACKAGE_VERSION
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
project(greet VERSION ${PACKAGE_VERSION})

add_library(greet SHARED "src/lib/greet.c")
include_directories(greet PUBLIC include)

add_executable(hello "src/cli/main.c")
target_link_libraries(hello greet)

install(
    TARGETS greet
    COMPONENT libraries
    DESTINATION "lib"
)

install(
    TARGETS hello
    COMPONENT applications
    DESTINATION "bin"
)

install(
    FILES "include/greet.h"
    DESTINATION "include"
    COMPONENT headers
)

include(deb)  # build/cmake/deb.cmake
include(CPack)
