add_executable(testcms testcms2.c testplugin.c zoo_icc.c)
target_link_libraries(testcms PUBLIC lcms2 lcms2_fast_float)
target_compile_definitions(testcms PRIVATE LCMS_FAST_EXTENSIONS=1)
