set(lcms_SRCS
    cmscnvrt.c
    cmserr.c
    cmsgamma.c
    cmsgmt.c
    cmsintrp.c
    cmsio0.c
    cmsio1.c
    cmslut.c
    cmsplugin.c
    cmssm.c
    cmsmd5.c
    cmsmtrx.c
    cmspack.c
    cmspcs.c
    cmswtpnt.c
    cmsxform.c
    cmssamp.c
    cmsnamed.c
    cmscam02.c
    cmsvirt.c
    cmstypes.c
    cmscgats.c
    cmsps2.c
    cmsopt.c
    cmshalf.c
    cmsalpha.c
)

if (MSVC)
    set(lcms_SRCS ${lcms_SRCS}
        lcms2.def
    )
endif()

if (BUILD_SHARED_LIBS)
    add_library(lcms2 SHARED ${lcms_SRCS})
else()
    add_library(lcms2 STATIC ${lcms_SRCS})
endif()

set_target_properties(lcms2 PROPERTIES VERSION ${lcms2_VERSION}
                                      SOVERSION ${lcms2_VERSION_MAJOR})

if(BUILD_SHARED_LIBS AND WIN32)
    target_compile_definitions(lcms2 PUBLIC CMS_DLL_BUILD=1)
elseif(NOT BUILD_SHARED_LIBS)
    target_compile_definitions(lcms2 PUBLIC CMS_STATIC=1)
endif()

if(HAVE_LIBM)
    target_link_libraries(lcms2 PUBLIC m)
endif()

if(HAVE_FUNC_ATTRIBUTE_VISIBILITY)
    target_compile_definitions(lcms2 PUBLIC HAVE_FUNC_ATTRIBUTE_VISIBILITY=1)
endif()

if(HAVE_VISIBILITY_HIDDEN)
    target_compile_options(lcms2 PUBLIC "-fvisibility=hidden")
endif()

if(Threads_FOUND)
    target_compile_definitions(lcms2 PRIVATE HasTHREADS=1)
    target_link_libraries(lcms2 PUBLIC Threads::Threads)
endif()

target_include_directories(lcms2 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/include>
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

install(TARGETS lcms2
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)

install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/../include/lcms2.h
    DESTINATION include
)
