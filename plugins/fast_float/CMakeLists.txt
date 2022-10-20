include(CheckCXXSourceCompiles)
check_cxx_source_compiles ("#include <emmintrin.h>
int main() { __m128i n = _mm_set1_epi8(42); }" HAVE_SSE2)

set(lcms_plugin_SRCS 
    src/fast_16_tethra.c
    src/fast_8_curves.c
    src/fast_8_matsh.c
    src/fast_8_tethra.c
    src/fast_float_15bits.c
    src/fast_float_15mats.c
    src/fast_float_cmyk.c
    src/fast_float_curves.c
    src/fast_float_lab.c
    src/fast_float_matsh.c
    src/fast_float_separate.c
    src/fast_float_sup.c
    src/fast_float_tethra.c
)

add_library(lcms2_fast_float SHARED ${lcms_plugin_SRCS})

target_include_directories(lcms2_fast_float PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

if (NOT HAVE_SSE)
    target_compile_definitions(lcms2_fast_float PUBLIC CMS_DONT_USE_SSE2=1)
endif()

set_target_properties(lcms2_fast_float PROPERTIES VERSION ${lcms2_VERSION}
                                      SOVERSION ${lcms2_VERSION_MAJOR})

if(WIN32)
    set_target_properties(lcms2_fast_float PROPERTIES COMPILE_FLAGS -DCMS_DLL_BUILD=1)
endif()

target_link_libraries(lcms2_fast_float PUBLIC lcms2)

install(TARGETS lcms2_fast_float RUNTIME DESTINATION bin
                                LIBRARY DESTINATION lib
                                ARCHIVE DESTINATION lib)

install(FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/../../include/lcms2_plugin.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/lcms2_fast_float.h
    DESTINATION include
)
