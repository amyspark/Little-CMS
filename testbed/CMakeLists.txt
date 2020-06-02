include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../include ${CMAKE_CURRENT_SOURCE_DIR}/../src)

add_executable(testcms testcms2.c testplugin.c zoo_icc.c)
target_link_libraries(testcms lcms_static)
set_target_properties(testcms PROPERTIES COMPILE_FLAGS -DCMS_STATIC)
