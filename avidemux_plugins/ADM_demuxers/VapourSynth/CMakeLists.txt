include(admCheckVapourSynth)
include(dm_plugin)
checkVapourSynth()
IF(BLAH)
IF(USE_VAPOURSYNTH)
   IF(DO_COMMON)
        #ADD_DEFINITIONS("-g3 -O0")
        ADD_DEFINITIONS("-std=c++11")
 #       ADD_DEFINITIONS("-std=c++11")
        ADD_DEFINITIONS(${VAPOURSYNTHSCRIPT_CFLAGS})
        SET(SRCS ADM_vsProxy.cpp ADM_vsProxy_cli.cpp)
        ADD_EXECUTABLE(vsProxy ${SRCS})
        GET_PROPERTY(OLD 
                        TARGET vsProxy  
                        PROPERTY LINK_FLAGS)
        IF("${OLD}" MATCHES ".*NOT.*FOUND")
                SET(OLD "")
        ENDIF("${OLD}" MATCHES ".*NOT.*FOUND")
# Keep -L, discard everything else
        FOREACH(i ${VAPOURSYNTHSCRIPT_LDFLAGS})
           IF(${i} MATCHES "^-L")
                MESSAGE(STATUS "Updating linker flags  => ${i}")
                SET(OLD "${i} ${OLD} ") 
           ELSE(${i} MATCHES "^-L")
                MESSAGE(STATUS "Updating linker libs  => ${i}")
                LIST(APPEND LIBS_TO_LINK  ${i})
           ENDIF(${i} MATCHES "^-L")
        ENDFOREACH(i ${VAPOURSYNTHSCRIPT_LDFLAGS})
        # Add vs Proxy
        SET_TARGET_PROPERTIES(vsProxy PROPERTIES LINK_FLAGS "${OLD}")
        TARGET_LINK_LIBRARIES(vsProxy ${LIBS_TO_LINK} ADM_core6 ADM_coreSocket6)
        #----------------
        # Add direct Loading
        SET(ADM_vs_SRCS 
                ADM_vs.cpp  ADM_vs.h  ADM_vsPlugin.cpp  
                )
        #INIT_DEMUXER(ADM_dm_vapoursynth)
        #ADD_DEMUXER(ADM_dm_vapoursynth ${ADM_vs_SRCS})
        #TARGET_LINK_LIBRARIES(ADM_dm_vapoursynth ${LIBS_TO_LINK} ADM_core6 ADM_coreSocket6)
        #INSTALL_DEMUXER(ADM_dm_vapoursynth)
        #----------------
        iF( WIN32)
                TARGET_LINK_LIBRARIES(vsProxy -lwsock32 winmm -mwindows -lm -lstdc++)
        else( WIN32)
                TARGET_LINK_LIBRARIES(vsProxy -lstdc++) # clang
        ENDif( WIN32)
        ADM_INSTALL_BIN(vsProxy)
   ENDIF(DO_COMMON)
   IF(DO_QT4)
        ADD_SUBDIRECTORY(qt4)
   ENDIF(DO_QT4)
ENDIF(USE_VAPOURSYNTH)
ENDIF(BLAH)
