# test/CMakeLists.txt

function(ntk_example target)
    add_executable(${target} ${ARGN})

    target_include_directories(${target} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/..
    )

    target_link_libraries(${target}
        ntk_static
        ntk_images_static
        PkgConfig::X11
        PkgConfig::FONTCONFIG
        PkgConfig::XFT
        PkgConfig::CAIRO
    )
endfunction()

function(ntk_example_gl target)
    add_executable(${target} ${ARGN})

    target_include_directories(${target} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/..
    )

    target_link_libraries(${target}
        ntk_static
        ntk_images_static
        ntk_gl_static
    )
endfunction()


# --------------------------------------------------
# Non-GL Examples
# --------------------------------------------------

ntk_example(embed embed.cxx)
ntk_example(hello hello.cxx)
ntk_example(input_choice input_choice.cxx)
ntk_example(label label.cxx)
ntk_example(native-filechooser native-filechooser.cxx)
ntk_example(fonts fonts.cxx)
ntk_example(editor editor.cxx)
ntk_example(buttons buttons.cxx)
ntk_example(sudoku sudoku.cxx)
ntk_example(ask ask.cxx)
ntk_example(pixmap pixmap.cxx)
ntk_example(cursor cursor.cxx)
ntk_example(line_style line_style.cxx)
ntk_example(adjuster adjuster.cxx)
ntk_example(list_visuals list_visuals.cxx)
ntk_example(subwindow subwindow.cxx)
ntk_example(resize resize.cxx)
ntk_example(image image.cxx)
ntk_example(file_chooser file_chooser.cxx)
ntk_example(tiled_image tiled_image.cxx)
ntk_example(navigation navigation.cxx)
ntk_example(device device.cxx)
ntk_example(utf8 utf8.cxx)
ntk_example(overlay overlay.cxx)
ntk_example(arc arc.cxx)
ntk_example(cairo_test cairo_test.cxx)
ntk_example(browser browser.cxx)
ntk_example(colbrowser colbrowser.cxx)
ntk_example(rotated_text rotated_text.cxx)
ntk_example(inactive inactive.cxx)
ntk_example(threads threads.cxx)
ntk_example(demo demo.cxx)
ntk_example(menubar menubar.cxx)
ntk_example(input input.cxx)
ntk_example(message message.cxx)
ntk_example(preferences preferences.cxx)
ntk_example(tree tree.cxx)
ntk_example(output output.cxx)
ntk_example(scroll scroll.cxx)
ntk_example(symbols symbols.cxx)
ntk_example(tabs tabs.cxx)
ntk_example(blocks blocks.cxx)
ntk_example(keyboard keyboard.cxx keyboard_ui.cxx)
ntk_example(iconize iconize.cxx)
ntk_example(radio radio.cxx)
ntk_example(resizebox resizebox.cxx)
ntk_example(fast_slow fast_slow.cxx)
ntk_example(boxtype boxtype.cxx)
ntk_example(help_dialog help.cxx)
ntk_example(bitmap bitmap.cxx)
ntk_example(tile tile.cxx)
ntk_example(color_chooser color_chooser.cxx)
ntk_example(valuators valuators.cxx)
ntk_example(doublebuffer doublebuffer.cxx)
ntk_example(pixmap_browser pixmap_browser.cxx)
ntk_example(curve curve.cxx)
ntk_example(minimum minimum.cxx)
ntk_example(button button.cxx)
ntk_example(pack pack.cxx)
ntk_example(checkers checkers.cxx)
ntk_example(table table.cxx)
ntk_example(mandelbrot mandelbrot.cxx mandelbrot_ui.cxx)
ntk_example(fractals fractals.cxx fracviewer.cxx)


# --------------------------------------------------
# GL Examples (only if GL enabled)
# --------------------------------------------------

if(ENABLE_GL)
    ntk_example_gl(glpuzzle glpuzzle.cxx)
    ntk_example_gl(gl_overlay gl_overlay.cxx)
    ntk_example_gl(shape shape.cxx)
    ntk_example_gl(fullscreen fullscreen.cxx)
    ntk_example_gl(cube cube.cxx)
endif()
