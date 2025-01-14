set(THPOOL_VERSION "0.0.1")

set_version(THPOOL_VERSION THPOOL_VERSION_MAJOR THPOOL_VERSION_MINOR THPOOL_VERSION_PATCH THPOOL_VERSION_REVISION)

set(
    thpool_SOURCES
    thpool.c
)

include(GNUInstallDirs)

add_library(thpool STATIC ${thpool_SOURCES})
set_target_properties(thpool
        PROPERTIES OUTPUT_NAME thpool
                   POSITION_INDEPENDENT_CODE ON        
)
install(TARGETS thpool
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

if(BUILD_SHARED_LIBS)
    add_library(thpool_shared SHARED ${thpool_SOURCES})
    set_target_properties(
        thpool_shared
        PROPERTIES OUTPUT_NAME thpool
                   POSITION_INDEPENDENT_CODE ON
                   SOVERSION "${THPOOL_VERSION_MAJOR}"
                   VERSION "${THPOOL_VERSION}")
    
    install(TARGETS thpool_shared
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    )
endif()

# Install headers
install(DIRECTORY "${PROJECT_SOURCE_DIR}/include/thpool"
        DESTINATION "include"
)

configure_file(${PROJECT_SOURCE_DIR}/libthpool.pc.in ${CMAKE_BINARY_DIR}/libthpool.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/libthpool.pc DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)

if(BUILD_TESTING)
   add_subdirectory(test)
endif() # END TEST
