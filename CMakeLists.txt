cmake_minimum_required(VERSION 3.1)

project(lcms2 VERSION "2.12")

option(BUILD_TESTS "build the test executable" OFF)
option(BUILD_STATIC "build the static library" OFF)
option(BUILD_UTILS "build the utilities executables" OFF)
option(BUILD_PLUGINS "build the plugins" ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

include_directories(
${CMAKE_BINARY_DIR}/include 
${CMAKE_SOURCE_DIR}/include
)

include(CheckLibraryExists)
CHECK_LIBRARY_EXISTS(m sqrt "" HAVE_LIBM)

include(FindThreads)

add_subdirectory(src)

if(BUILD_PLUGINS)
    add_subdirectory(plugins/fast_float)
endif()

if(BUILD_TESTS)
    add_subdirectory(testbed)
endif(BUILD_TESTS)

if(BUILD_UTILS)
    add_subdirectory(utils)
endif(BUILD_UTILS)

set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix "\${prefix}")
set(libdir "\${prefix}/lib")
set(includedir "\${exec_prefix}/include")
set(PACKAGE ${PROJECT_NAME})
set(VERSION ${lcms2_VERSION})
if(BUILD_PLUGINS)
    set(LIB_PLUGINS "-lfast_float_plugin")
endif()
if(HAVE_LIBM)
    set(LIB_MATH "-lm")
endif()
if(Threads_FOUND)
    set(LIB_THREAD ${CMAKE_THREAD_LIBS_INIT})
endif()

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/lcms2.pc.in ${CMAKE_CURRENT_BINARY_DIR}/lcms2.pc @ONLY)
install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/lcms2.pc
    DESTINATION lib/pkgconfig
)
