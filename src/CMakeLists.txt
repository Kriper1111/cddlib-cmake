list(APPEND bin_PROGRAMS scdd
    lcdd
    redcheck
    fourier
    projection
    adjacency
    allfaces
    testcdd1
    testshoot
    testcdd2
    testlp1
    testlp2
    testlp3
    cddexec
)

list(APPEND noinst_PROGRAMS redexter
    redundancies
    redundancies_clarkson
)

foreach(program_name ${bin_PROGRAMS})
    add_executable(${program_name} "${CMAKE_SOURCE_DIR}/src/${program_name}.c")
    target_link_libraries(${program_name} cdd)
endforeach(program_name ${bin_PROGRAMS})

foreach(program_name ${noinst_PROGRAMS})
    add_executable(${program_name} "${CMAKE_SOURCE_DIR}/src/${program_name}.c")
    target_link_libraries(${program_name} cdd)
endforeach(program_name ${noinst_PROGRAMS})

install(TARGETS ${bin_PROGRAMS})

include_directories("${CMAKE_SOURCE_DIR}/lib-src")

message("bin_PROGRAMS = ${bin_PROGRAMS}")
message("noinst_PROGRAMS = ${noinst_PROGRAMS}")