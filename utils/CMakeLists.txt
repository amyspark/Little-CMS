include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/common
    ${CMAKE_SOURCE_DIR}/include # override search path if LCMS is already installed
)

find_package(JPEG)
find_package(TIFF)

set(UTILS_EXECUTABLES )
set(UTILS_MANPAGES )

###############################################################################
if(JPEG_FOUND)
    set(JPGICC_SRCS
        jpgicc/jpgicc.c
        jpgicc/iccjpeg.c
        common/xgetopt.c
        common/vprf.c
    )
    add_executable(jpgicc ${JPGICC_SRCS})
    target_link_libraries(jpgicc PUBLIC lcms2 ${JPEG_LIBRARIES})
    target_include_directories(jpgicc AFTER PRIVATE ${JPEG_INCLUDE_DIR})
    list(APPEND UTILS_EXECUTABLES jpgicc)
    list(APPEND UTILS_MANPAGES jpgicc/jpgicc.1)
endif(JPEG_FOUND)

###############################################################################
set(LINKICC_SRCS
    linkicc/linkicc.c
    common/xgetopt.c
    common/vprf.c
)
add_executable(linkicc ${LINKICC_SRCS})
target_link_libraries(linkicc PUBLIC lcms2)
list(APPEND UTILS_EXECUTABLES linkicc)
list(APPEND UTILS_MANPAGES linkicc/linkicc.1)

###############################################################################
set(PSICC_SRCS
    psicc/psicc.c
    common/xgetopt.c
    common/vprf.c
)
add_executable(psicc ${PSICC_SRCS})
target_link_libraries(psicc PUBLIC lcms2)
list(APPEND UTILS_EXECUTABLES psicc)
list(APPEND UTILS_MANPAGES psicc/psicc.1)

###############################################################################
if(TIFF_FOUND)
    set(TIFICC_SRCS
        tificc/tificc.c
        common/xgetopt.c
        common/vprf.c
    )
    add_executable(tificc ${TIFICC_SRCS})
    target_link_libraries(tificc PUBLIC lcms2 ${TIFF_LIBRARIES})
    target_include_directories(tificc AFTER PRIVATE ${TIFF_INCLUDE_DIR})
    list(APPEND UTILS_EXECUTABLES tificc)
    list(APPEND UTILS_MANPAGES tificc/tificc.1)
endif(TIFF_FOUND)

###############################################################################
set(TRANSICC_SRCS
    transicc/transicc.c
    common/xgetopt.c
    common/vprf.c
)
add_executable(transicc ${TRANSICC_SRCS})
target_link_libraries(transicc PUBLIC lcms2)
list(APPEND UTILS_EXECUTABLES transicc)
list(APPEND UTILS_MANPAGES transicc/transicc.1)

install(TARGETS ${UTILS_EXECUTABLES} RUNTIME DESTINATION bin
                                     LIBRARY DESTINATION lib
                                     ARCHIVE DESTINATION lib)
install(FILES ${UTILS_MANPAGES} DESTINATION share/man/man1)
