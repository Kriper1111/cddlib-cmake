set(libcdd_SOURCES cddcore.c cddlp.c cddmp.c cddio.c
    cddlib.c cddproj.c setoper.c
)

set(libcdd_HEADERS cdd.h cddmp.h cddtypes.h
    setoper.h splitmix64.h
)

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

message("libcdd_SOURCES = ${libcdd_SOURCES}")
message("libcdd_HEADERS = ${libcdd_HEADERS}")

add_library(cdd ${libcdd_SOURCES} ${libcdd_HEADERS})
target_compile_options(cdd PRIVATE -UGMPRATIONAL)

install(TARGETS cdd)
install(FILES ${libcdd_HEADERS} DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/cddlib")
install(FILES ${libcdd_HEADERS} DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/cdd")

# XXX: cdd <-> gmp compat needs attention:
#    1) it needs sed, so we can't 100% go away from gnu tools
#    2) it needs sed, which is barely usable from cmake targets as quotes go all over the place
#    3) it needs sed, which is using pipes, which will make it even more *tricky* to use with CMake
# conclusion: the root of all evil is sed and autotools.
# if(HAS_GMP_H)
#     target_compile_definitions(cdd PUBLIC GMPRATIONAL)

#     set(libcddgmp_HEADERS_PREREQ cdd.h cddmp.h cddtypes.h)
#     set(libcddgmp_HEADERS_PREREQ cddcore.c cddlp.c cddmp.c cddio.c cddlib.c cddproj.c)

#     set(libcddgmp_HEADERS cdd_f.h cddmp_f.h cddtypes_f.h)
#     set(libcddgmp_SOURCES cddcore_f.c cddlp_f.c cddmp_f.c cddio_f.c cddlib_f.c cddproj_f.c)

#     add_custom_command(OUTPUT ${libcddgmp_SOURCES} ${libcddgmp_HEADERS}
#         DEPENDS ${libcddgmp_SOURCES_PREREQ} ${libcddgmp_HEADERS_PREREQ}
#         COMMAND bash -c "${CMAKE_SOURCE_DIR}/lib-src/make_gmp.sh"
#     )
#     add_library(cddgmp SHARED ${libcdd_SOURCES} ${libcdd_HEADERS})
# endif(HAS_GMP_H)