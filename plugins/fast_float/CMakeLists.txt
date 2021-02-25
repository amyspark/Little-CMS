
if (BUILD_PLUGINS)
    include(CheckCXXSourceCompiles)
    check_cxx_source_compiles ("
        #include <immintrin.h>
        int main ()
        {
            __m128d a, b;
            double vals[2] = {0};
            a = _mm_loadu_pd (vals);
            b = _mm_add_pd (a,a);
            _mm_storeu_pd (vals,b);
            return (0);
        }"
        HAVE_SSE2)

    if (NOT HAVE_SSE2)
        message(STATUS "Disabling SSE optimizations, as the target doesn't support them")
        file(READ ${CMAKE_CURRENT_SOURCE_DIR}/include/lcms2_fast_float.h tmp)
        string(REPLACE "//  #define CMS_DONT_USE_SSE2 1" "#define CMS_DONT_USE_SSE2 1" tmp2 "${tmp}")
        file(WRITE ${CMAKE_CURRENT_SOURCE_DIR}/include/lcms2_fast_float.h "${tmp2}")
    endif()

    set(lcms_plugin_SRCS 
        src/fast_16_tethra.c
        src/fast_8_curves.c
        src/fast_8_matsh.c
        src/fast_8_matsh_sse.c
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

    include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

    add_library(lcms2_fast_float SHARED ${lcms_plugin_SRCS})
    if(WIN32)
       set_target_properties(lcms2_fast_float PROPERTIES DEFINE_SYMBOL CMS_DLL_BUILD)
    endif(WIN32)

    target_link_libraries(lcms2_fast_float PUBLIC lcms)

    install(TARGETS lcms2_fast_float RUNTIME DESTINATION bin
                                    LIBRARY DESTINATION lib
                                    ARCHIVE DESTINATION lib)

    install(FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/../../include/lcms2_plugin.h
        ${CMAKE_CURRENT_SOURCE_DIR}/include/lcms2_fast_float.h
        DESTINATION include)
endif()
