cmake_minimum_required(VERSION 3.10)

project(cddlib VERSION 0.94.13)

include(GNUInstallDirs)
set(libcdd_version_info "1:2:1")
# XXX: m4 macros?

include(CheckIncludeFile)
# include(CMakePackageConfigHelpers)

if(WIN32)
    add_link_options(-no-undefined)
endif(WIN32)

set(CDD_LIBS cdd)

CHECK_INCLUDE_FILE("gmp.h" HAS_GMP_H)
if(HAS_GMP_H)
    list(APPEND CDD_LIBS cddgmp)
endif(HAS_GMP_H)

add_subdirectory(doc)
add_subdirectory(lib-src)
add_subdirectory(src)