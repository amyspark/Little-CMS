project(lcms2)

option(BUILD_TESTS "build the test executable" OFF)
option(BUILD_STATIC "build the static library" OFF)
option(BUILD_UTILS "build the utilities executables" OFF)
option(BUILD_PLUGINS "build the plugins" ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

cmake_minimum_required(VERSION 2.6)

include_directories(
${CMAKE_BINARY_DIR}/include 
${CMAKE_SOURCE_DIR}/include
)

add_subdirectory(src)

if(BUILD_TESTS)
    add_subdirectory(testbed)
endif(BUILD_TESTS)

if(BUILD_UTILS)
    add_subdirectory(utils)
endif(BUILD_UTILS)
