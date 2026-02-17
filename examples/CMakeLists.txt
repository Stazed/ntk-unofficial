# example/CMakeLists.txt

if(ENABLE_EXAMPLES_STATIC)
    function(ntk_example target)
        add_executable(${target} ${ARGN})

        target_include_directories(${target} PRIVATE
            ${CMAKE_CURRENT_SOURCE_DIR}
            ${CMAKE_CURRENT_SOURCE_DIR}/..
        )

        target_link_libraries(${target}
            ntk_static
            ntk_images_static
            ${X11_LIBRARIES}
            ${FONTCONFIG_LIBRARIES}
            ${CAIRO_LIBRARIES}
        )
    endfunction()
else()  # Dynamic
    function(ntk_example target)
        add_executable(${target} ${ARGN})

        target_include_directories(${target} PRIVATE
            ${CMAKE_CURRENT_SOURCE_DIR}
            ${CMAKE_CURRENT_SOURCE_DIR}/..
        )

        target_link_libraries(${target}
            ntk
            ntk_images
            ${X11_LIBRARIES}
        )
    endfunction()
endif() #ENABLE_EXAMPLES_STATIC

ntk_example(howto-add_fd-and-popen howto-add_fd-and-popen.cxx)
ntk_example(howto-drag-and-drop howto-drag-and-drop.cxx)
ntk_example(howto-parse-args howto-parse-args.cxx)
ntk_example(howto-text-over-image-button howto-text-over-image-button.cxx)
ntk_example(menubar-add menubar-add.cxx)
ntk_example(nativefilechooser-simple-app nativefilechooser-simple-app.cxx)
ntk_example(table-as-container table-as-container.cxx)
ntk_example(table-simple table-simple.cxx)
ntk_example(table-sort table-sort.cxx)
ntk_example(table-spreadsheet table-spreadsheet.cxx)
ntk_example(table-spreadsheet-with-keyboard-nav table-spreadsheet-with-keyboard-nav.cxx)
ntk_example(tabs-simple tabs-simple.cxx)
ntk_example(textdisplay-with-colors textdisplay-with-colors.cxx)
ntk_example(texteditor-simple texteditor-simple.cxx)
ntk_example(tree-simple tree-simple.cxx)
ntk_example(wizard-simple wizard-simple.cxx)
