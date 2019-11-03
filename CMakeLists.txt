cmake_minimum_required(VERSION 3.10.2)

set(CMAKE_BUILD_TYPE Release)

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
    DESTINATION "/usr/lib"
)

install(
    TARGETS hello
    COMPONENT applications
    DESTINATION "/usr/bin"
)

set(CPACK_GENERATOR "DEB")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Yan QiDong")
set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "This is a demo package from Yan QiDong.")
set(CPACK_DEBIAN_PACKAGE_VERSION ${PACKAGE_VERSION})
set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
include(CPack)
