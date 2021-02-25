# some configure checks:
if(WIN32)
    if(MSVC)
        set(UINT8_T "unsigned char")
        set(UINT16_T "unsigned short")
        set(UINT32_T "unsigned long")
        set(INT8_T "char")
        set(INT16_T "short")
        set(INT32_T "long")
    else(MSVC)
        set(UINT8_T "uint8_t")
        set(UINT16_T "uint16_t")
        set(UINT32_T "uint32_t")
        set(INT8_T "int8_t")
        set(INT16_T "int16_t")
        set(INT32_T "int32_t")
    endif(MSVC)
endif(WIN32)

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

add_library(lcms SHARED ${lcms_SRCS})
set_target_properties(lcms PROPERTIES OUTPUT_NAME "lcms2"
                                      VERSION ${lcms2_VERSION}
                                      SOVERSION ${lcms2_VERSION_MAJOR}
                                      DEFINE_SYMBOL CMS_DLL_BUILD)

if(HAVE_LIBM)
    target_link_libraries(lcms PRIVATE m)
endif()
if(Threads_FOUND)
    target_link_libraries(lcms PRIVATE Threads::Threads)
endif()

if(BUILD_TESTS OR BUILD_UTILS OR BUILD_STATIC)
    add_library(lcms_static ${lcms_SRCS})
    set_target_properties(lcms_static PROPERTIES COMPILE_FLAGS -DCMS_STATIC)
    set(LCMS_STATIC lcms_static)
endif(BUILD_TESTS OR BUILD_UTILS OR BUILD_STATIC)

install(TARGETS lcms ${LCMS_STATIC} RUNTIME DESTINATION bin
                                    LIBRARY DESTINATION lib
                                    ARCHIVE DESTINATION lib)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/../include/lcms2.h
    DESTINATION include
)
