
find_package(Doxygen REQUIRED)

if(DOXYGEN_FOUND AND BUILD_DOC_HTML)
    set(DOXYFILE_IN  ${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile)
    set(DOXYFILE_OUT ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile)

    configure_file(${DOXYFILE_IN} ${DOXYFILE_OUT} @ONLY)

    add_custom_target(html
        COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYFILE_OUT}
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
                ${SRC_DOCDIR}/tiny.png
                ${CMAKE_CURRENT_BINARY_DIR}/html/tiny.png
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating HTML documentation"
    )
endif()

#if(DOXYGEN_FOUND AND BUILD_DOC_PDF)
#    find_program(PDFLATEX_EXECUTABLE pdflatex)
#    find_program(MAKEINDEX_EXECUTABLE makeindex)

#    add_custom_target(pdf
#        COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYFILE_OUT}
#        COMMAND ${PDFLATEX_EXECUTABLE} --interaction=nonstopmode refman.tex
#        COMMAND ${MAKEINDEX_EXECUTABLE} refman.idx
#        COMMAND ${PDFLATEX_EXECUTABLE} --interaction=nonstopmode refman.tex
#        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/latex
#        COMMENT "Generating PDF documentation"
#    )
#endif()

if(DOXYGEN_FOUND)
    add_custom_command(
        OUTPUT ${CMAKE_BINARY_DIR}/documentation/.docs_copied
        COMMAND ${CMAKE_COMMAND} -E copy_directory
                ${CMAKE_SOURCE_DIR}/documentation/src
                ${CMAKE_BINARY_DIR}/documentation/src
        COMMAND ${CMAKE_COMMAND} -E touch
                ${CMAKE_BINARY_DIR}/documentation/.docs_copied
        COMMENT "Copying documentation sources"
    )

    add_custom_target(copy_docs ALL
        DEPENDS ${CMAKE_BINARY_DIR}/documentation/.docs_copied
)
endif()

add_custom_target(doc ALL
    COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_OUT}
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

add_dependencies(doc copy_docs)