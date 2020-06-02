include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../include
    ${CMAKE_CURRENT_SOURCE_DIR}/../src
    ${CMAKE_CURRENT_SOURCE_DIR}/../utils/common
)

find_package(JPEG)
find_package(TIFF)

set(UTILS_EXECUTABLES )
set(UTILS_MANPAGES )

###############################################################################
if(JPEG_FOUND)
    include_directories(${JPEG_INCLUDE_DIR})
    set(JPGICC_SRCS
        jpgicc/jpgicc.c
        jpgicc/iccjpeg.c
        common/xgetopt.c
        common/vprf.c
    )
    add_executable(jpgicc ${JPGICC_SRCS})
    target_link_libraries(jpgicc lcms ${JPEG_LIBRARIES})
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
target_link_libraries(linkicc lcms)
list(APPEND UTILS_EXECUTABLES linkicc)
list(APPEND UTILS_MANPAGES linkicc/linkicc.1)

###############################################################################
set(PSICC_SRCS
    psicc/psicc.c
    common/xgetopt.c
    common/vprf.c
)
add_executable(psicc ${PSICC_SRCS})
target_link_libraries(psicc lcms)
list(APPEND UTILS_EXECUTABLES psicc)
list(APPEND UTILS_MANPAGES psicc/psicc.1)

###############################################################################
if(TIFF_FOUND)
    include_directories(${TIFF_INCLUDE_DIR})
    set(JPGICC_SRCS
        tificc/tificc.c
        common/xgetopt.c
        common/vprf.c
    )
    add_executable(tificc ${JPGICC_SRCS})
    target_link_libraries(tificc lcms ${TIFF_LIBRARIES})
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
target_link_libraries(transicc lcms)
list(APPEND UTILS_EXECUTABLES transicc)
list(APPEND UTILS_MANPAGES transicc/transicc.1)

install(TARGETS ${UTILS_EXECUTABLES} RUNTIME DESTINATION bin
                                     LIBRARY DESTINATION lib
                                     ARCHIVE DESTINATION lib)
install(FILES ${UTILS_MANPAGES} DESTINATION share/man/man1)
