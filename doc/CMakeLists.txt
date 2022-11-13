find_program(LATEX latex)
find_program(PDF_LATEX pdflatex)
find_program(LATEX_TO_HTML latex2html)
find_program(DVIPS dvips)

set(DOCS_DIR "${CMAKE_SOURCE_DIR}/doc")

if(LATEX)
    list(APPEND LATEX_SRC
        "${DOCS_DIR}/cddlibman.tex"
        "${DOCS_DIR}/cddlibman.bbl"
    )

    message("Found LaTeX at ${LATEX}")

    add_custom_command(OUTPUT "${DOCS_DIR}/cddlibman.dvi"
        DEPENDS ${LATEX_SRC}
        COMMAND "${LATEX}" ${LATEX_SRC}
        WORKING_DIRECTORY "${DOCS_DIR}"
        VERBATIM
    )
    add_custom_target(docs-latex ALL
        DEPENDS "${DOCS_DIR}/cddlibman.dvi"
        COMMENT "Building latex docs"
    )
    install(
        FILES "${DOCS_DIR}/cddlibman.dvi"
        DESTINATION "${CMAKE_INSTALL_DOCDIR}/cddlib"
    )
endif(LATEX)

if(PDF_LATEX)
    set(PDF_LATEX_SRC
        "${DOCS_DIR}/cddlibman.tex"
        "${DOCS_DIR}/cddlibman.bbl"
    )

    message("Found pdflatex at ${PDF_LATEX}")

    add_custom_command(OUTPUT "${DOCS_DIR}/cddlibman.pdf"
        DEPENDS "${PDF_LATEX_SRC}"
        COMMAND "${PDF_LATEX}" "${PDF_LATEX_SRC}"
        WORKING_DIRECTORY ${DOCS_DIR}
    )
    add_custom_target(docs-pdflatex
        DEPENDS "${DOCS_DIR}/cddlibman.pdf"
        COMMENT "Building pdf docs"
    )

    install(
        FILES "${DOCS_DIR}/cddlibman.pdf"
        DESTINATION "${CMAKE_INSTALL_DOCDIR}/cddlib"
    )
endif(PDF_LATEX)

if(DVIPS)
    message("Found dvips at ${DVIPS}")

    add_custom_command(OUTPUT "${DOCS_DIR}/cddlibman.ps"
        COMMAND "${DVIPS}" "${DOCS_DIR}/cddlibman.dvi"
        DEPENDS "${DOCS_DIR}/cddlibman.dvi"
        WORKING_DIRECTORY ${DOCS_DIR}
    )
    add_custom_target(docs-dvips
        DEPENDS "${DOCS_DIR}/cddlibman.ps"
        COMMENT "Building .ps docs"
    )
    install(
        FILES "${DOCS_DIR}/cddlibman.ps"
        DESTINATION "${CMAKE_INSTALL_DOCDIR}/cddlib"
    )
endif(DVIPS)

if(LATEX_TO_HTML)
    set(LATEX_TO_HTML_SRC
        "${DOCS_DIR}/cddlibman.tex"
        "${DOCS_DIR}/cddlibman.bbl"
        "${DOCS_DIR}/html.sty"
        "${DOCS_DIR}/cddlibman.dvi"
    )

    message("Found latex2html at ${LATEX_TO_HTML}")

    add_custom_command(OUTPUT "${DOCS_DIR}/cddlibman/cddlibman.html"
        DEPENDS "${LATEX_TO_HTML_SRC}"
        COMMAND "${CMAKE_COMMAND}" -E rm -rf cddlibman
        COMMAND "${LATEX_TO_HTML}" "${LATEX_TO_HTML_SRC}"
        WORKING_DIRECTORY ${DOCS_DIR}
    )
    add_custom_target(docs-latex2html
        DEPENDS "${DOCS_DIR}/cddlibman/cddlibman.html"
        COMMENT "Building HTML docs"
    )
endif(LATEX_TO_HTML)