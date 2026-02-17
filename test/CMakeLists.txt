# test/CMakeLists.txt

if(ENABLE_TEST_STATIC)
    function(ntk_test target)
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
    #        PkgConfig::XFT
        )
    endfunction()

    if(ENABLE_GL)
        function(ntk_test_gl target)
            add_executable(${target} ${ARGN})

            target_include_directories(${target} PRIVATE
                ${CMAKE_CURRENT_SOURCE_DIR}
                ${CMAKE_CURRENT_SOURCE_DIR}/..
            )

            target_link_libraries(${target}
                ntk_gl_static
            )
        endfunction()
    endif() #ENABLE_GL
else()  # Dynamic
    function(ntk_test target)
        add_executable(${target} ${ARGN})

        target_include_directories(${target} PRIVATE
            ${CMAKE_CURRENT_SOURCE_DIR}
            ${CMAKE_CURRENT_SOURCE_DIR}/..
        )

        target_link_libraries(${target}
            ntk
            ntk_images
            ${X11_LIBRARIES}
    #        ${FONTCONFIG_LIBRARIES}
    #        ${CAIRO_LIBRARIES}
    #        PkgConfig::XFT
        )
    endfunction()

    if(ENABLE_GL)
        function(ntk_test_gl target)
            add_executable(${target} ${ARGN})

            target_include_directories(${target} PRIVATE
                ${CMAKE_CURRENT_SOURCE_DIR}
                ${CMAKE_CURRENT_SOURCE_DIR}/..
            )

            target_link_libraries(${target}
                ntk_gl
                ntk_images
            )
        endfunction()
    endif() #ENABLE_GL
endif() #ENABLE_TEST_STATIC

function(ntk_copy_runtime target file)
    add_custom_command(TARGET ${target} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/${file}
            $<TARGET_FILE_DIR:${target}>/${file}
    )
endfunction()



# --------------------------------------------------
# Non-GL Examples
# --------------------------------------------------

ntk_test(embed embed.cxx)
ntk_test(hello hello.cxx)
ntk_test(input_choice input_choice.cxx)
ntk_test(label label.cxx)
ntk_test(native-filechooser native-filechooser.cxx)
ntk_test(fonts fonts.cxx)
ntk_test(editor editor.cxx)
ntk_test(buttons buttons.cxx)
ntk_test(sudoku sudoku.cxx)
ntk_test(ask ask.cxx)
ntk_test(pixmap pixmap.cxx)
ntk_test(cursor cursor.cxx)
ntk_test(line_style line_style.cxx)
ntk_test(adjuster adjuster.cxx)
ntk_test(list_visuals list_visuals.cxx)
ntk_test(subwindow subwindow.cxx)
ntk_test(resize resize.cxx)
ntk_test(image image.cxx)
ntk_test(file_chooser file_chooser.cxx)
ntk_test(tiled_image tiled_image.cxx)
ntk_test(navigation navigation.cxx)
ntk_test(device device.cxx)
ntk_test(utf8 utf8.cxx)
ntk_test(overlay overlay.cxx)
ntk_test(arc arc.cxx)
#ntk_test(cairo_test cairo_test.cxx) // this does not work for NTK
ntk_test(browser browser.cxx)
ntk_test(colbrowser colbrowser.cxx)
ntk_test(rotated_text rotated_text.cxx)
ntk_test(inactive inactive.cxx)
ntk_test(threads threads.cxx)
ntk_test(demo demo.cxx)
ntk_test(menubar menubar.cxx)
ntk_test(input input.cxx)
ntk_test(message message.cxx)
ntk_test(preferences preferences.cxx)
ntk_test(tree tree.cxx)
ntk_test(output output.cxx)
ntk_test(scroll scroll.cxx)
ntk_test(symbols symbols.cxx)
ntk_test(tabs tabs.cxx)
ntk_test(blocks blocks.cxx)
ntk_test(keyboard keyboard.cxx keyboard_ui.cxx)
ntk_test(iconize iconize.cxx)
ntk_test(radio radio.cxx)
ntk_test(resizebox resizebox.cxx)
ntk_test(fast_slow fast_slow.cxx)
ntk_test(boxtype boxtype.cxx)
ntk_test(help_dialog help.cxx)
ntk_test(bitmap bitmap.cxx)
ntk_test(tile tile.cxx)
ntk_test(color_chooser color_chooser.cxx)
ntk_test(valuators valuators.cxx)
ntk_test(doublebuffer doublebuffer.cxx)
ntk_test(pixmap_browser pixmap_browser.cxx)
ntk_test(curve curve.cxx)
ntk_test(minimum minimum.cxx)
ntk_test(button button.cxx)
ntk_test(pack pack.cxx)
ntk_test(checkers checkers.cxx)
ntk_test(table table.cxx)
ntk_test(mandelbrot mandelbrot.cxx mandelbrot_ui.cxx)

ntk_copy_runtime(demo demo.menu)
ntk_copy_runtime(browser browser.cxx)
ntk_copy_runtime(colbrowser rgb.txt)
ntk_copy_runtime(help_dialog help_dialog.html)

add_custom_command(TARGET help_dialog POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_CURRENT_SOURCE_DIR}/images
        $<TARGET_FILE_DIR:help_dialog>/images
)

# --------------------------------------------------
# GL Examples (only if GL enabled)
# --------------------------------------------------

if(ENABLE_GL)
    ntk_test_gl(glpuzzle glpuzzle.cxx)
    ntk_test_gl(gl_overlay gl_overlay.cxx)
    ntk_test_gl(shape shape.cxx)
    ntk_test_gl(fullscreen fullscreen.cxx)
    ntk_test_gl(cube cube.cxx)
    ntk_test_gl(fractals fractals.cxx fracviewer.cxx)
endif()
