# --------------------------------------------------
# ntk-fluid program
# --------------------------------------------------

set(NTK_FLUID_SOURCES
    CodeEditor.cxx
    Fl_Function_Type.cxx
    Fl_Group_Type.cxx
    Fl_Menu_Type.cxx
    Fl_Type.cxx
    Fl_Widget_Type.cxx
    Fl_Window_Type.cxx
    Fluid_Image.cxx
    about_panel.cxx
    align_widget.cxx
    alignment_panel.cxx
    code.cxx
    factory.cxx
    file.cxx
    fluid.cxx
    function_panel.cxx
    template_panel.cxx
    undo.cxx
    widget_panel.cxx
)

add_executable(ntk-fluid ${NTK_FLUID_SOURCES})

target_compile_definitions(ntk-fluid PRIVATE
    FL_LIBRARY=1
    FL_INTERNALS=1
)


target_include_directories(ntk-fluid PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/..
)

target_link_libraries(ntk-fluid
    ntk
    ntk_images

    ${JPEG_LIBRARY}
    ${LIBPNG_LIBRARIES}
    ${ZLIB_LIBRARIES}
    ${DL_LIBRARY}
    ${M_LIBRARY}
    Threads::Threads

    PkgConfig::X11
    PkgConfig::FONTCONFIG
    PkgConfig::XFT
    PkgConfig::CAIRO
)
