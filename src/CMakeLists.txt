# --------------------------------------------------
# NTK Core Library
# --------------------------------------------------
project(ntk VERSION 1.3.1002 LANGUAGES C CXX)

set(NTK_SOURCES
Fl_Cairo_Graphics_Driver.cxx
Fl.cxx
Fl_Adjuster.cxx
Fl_Bitmap.cxx
Fl_Browser.cxx
Fl_Browser_.cxx
Fl_Browser_load.cxx
Fl_Box.cxx
Fl_Button.cxx
Fl_Chart.cxx
Fl_Check_Browser.cxx
Fl_Check_Button.cxx
Fl_Choice.cxx
Fl_Color_Chooser.cxx
Fl_Counter.cxx
Fl_Dial.cxx
Fl_Dial_Base.cxx
Fl_Device.cxx
Fl_Double_Window.cxx
Fl_File_Browser.cxx
Fl_File_Chooser.cxx
Fl_File_Chooser2.cxx
Fl_File_Icon.cxx
Fl_File_Input.cxx
Fl_Group.cxx
Fl_Help_View.cxx
Fl_Help_Dialog.cxx
Fl_Image.cxx
Fl_Input.cxx
Fl_Input_.cxx
Fl_Light_Button.cxx
Fl_Menu.cxx
Fl_Menu_.cxx
Fl_Menu_Bar.cxx
Fl_Sys_Menu_Bar.cxx
Fl_Menu_Button.cxx
Fl_Menu_Window.cxx
Fl_Menu_add.cxx
Fl_Menu_global.cxx
Fl_Multi_Label.cxx
Fl_Native_File_Chooser.cxx
Fl_Overlay_Window.cxx
Fl_Pack.cxx
Fl_Paged_Device.cxx
Fl_Panzoomer.cxx
Fl_Pixmap.cxx
Fl_Positioner.cxx
Fl_Preferences.cxx
Fl_Printer.cxx
Fl_Progress.cxx
Fl_Repeat_Button.cxx
Fl_Return_Button.cxx
Fl_Round_Button.cxx
Fl_Scroll.cxx
Fl_Scrollbar.cxx
Fl_Shared_Image.cxx
Fl_Single_Window.cxx
Fl_Slider.cxx
Fl_Table.cxx
Fl_Table_Row.cxx
Fl_Tabs.cxx
Fl_Text_Buffer.cxx
Fl_Text_Display.cxx
Fl_Text_Editor.cxx
Fl_Tile.cxx
Fl_Tiled_Image.cxx
Fl_Tree.cxx
Fl_Tree_Item.cxx
Fl_Tree_Item_Array.cxx
Fl_Tree_Prefs.cxx
Fl_Tooltip.cxx
Fl_Valuator.cxx
Fl_Value_Input.cxx
Fl_Value_Output.cxx
Fl_Value_Slider.cxx
Fl_Widget.cxx
Fl_Window.cxx
Fl_Window_fullscreen.cxx
Fl_Window_hotspot.cxx
Fl_Window_iconize.cxx
Fl_Wizard.cxx
Fl_XBM_Image.cxx
Fl_XPM_Image.cxx
Fl_abort.cxx
Fl_add_idle.cxx
Fl_arg.cxx
Fl_compose.cxx
Fl_display.cxx
Fl_get_key.cxx
Fl_get_system_colors.cxx
Fl_grab.cxx
Fl_lock.cxx
Fl_own_colormap.cxx
Fl_visual.cxx
Fl_x.cxx
filename_absolute.cxx
filename_expand.cxx
filename_ext.cxx
filename_isdir.cxx
filename_list.cxx
filename_match.cxx
filename_setext.cxx
fl_arc.cxx
fl_arci.cxx
fl_ask.cxx
fl_boxtype.cxx
fl_color.cxx
fl_cursor.cxx
fl_curve.cxx
fl_diamond_box.cxx
fl_dnd.cxx
fl_draw.cxx
Fl_Cairo.cxx
fl_draw_image.cxx
fl_draw_pixmap.cxx
fl_encoding_latin1.cxx
fl_encoding_mac_roman.cxx
fl_engraved_label.cxx
fl_file_dir.cxx
fl_font.cxx
fl_labeltype.cxx
fl_line_style.cxx
fl_open_uri.cxx
fl_oval_box.cxx
fl_overlay.cxx
fl_read_image.cxx
fl_rect.cxx
fl_round_box.cxx
fl_rounded_box.cxx
fl_set_font.cxx
fl_set_fonts.cxx
fl_scroll_area.cxx
fl_shadow_box.cxx
fl_shortcut.cxx
fl_show_colormap.cxx
fl_symbols.cxx
fl_vertex.cxx
screen_xywh.cxx
fl_utf8.cxx
Fl_Theme.cxx
Fl_Theme_Chooser.cxx
Cairo_Theme.cxx
Gleam_Theme.cxx
GTK_Theme.cxx
Clean_Theme.cxx
Crystal_Theme.cxx
Vector_Theme.cxx
Vector2_Theme.cxx
themes.cxx
ps_image.cxx
fl_utf.c
vsnprintf.c
xutf8/case.c
xutf8/is_right2left.c
xutf8/is_spacing.c
xutf8/keysym2Ucs.c
xutf8/utf8Input.c
xutf8/utf8Utils.c
xutf8/utf8Wrap.c
numericsort.c
flstring.c
)

add_library(ntk SHARED ${NTK_SOURCES})
add_library(ntk_static STATIC ${NTK_SOURCES})

target_compile_definitions(ntk PRIVATE FL_LIBRARY=1 FL_INTERNALS=1)
target_compile_definitions(ntk_static PRIVATE FL_LIBRARY=1 FL_INTERNALS=1)

target_link_libraries(ntk
    PRIVATE
        PkgConfig::X11
        PkgConfig::FONTCONFIG
        PkgConfig::XFT
        PkgConfig::CAIRO
        Threads::Threads
        ${DL_LIBRARY}
        ${M_LIBRARY}
)


set_target_properties(ntk PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1
)

# --------------------------------------------------
# ntk_images
# --------------------------------------------------

set(NTK_IMAGE_SOURCES
    fl_images_core.cxx
    Fl_BMP_Image.cxx
    Fl_File_Icon2.cxx
    Fl_GIF_Image.cxx
    Fl_Help_Dialog.cxx
    Fl_JPEG_Image.cxx
    Fl_PNG_Image.cxx
    Fl_PNM_Image.cxx
    filename_ext.cxx
)

add_library(ntk_images SHARED ${NTK_IMAGE_SOURCES})
add_library(ntk_images_static STATIC ${NTK_IMAGE_SOURCES})

target_link_libraries(ntk_images
    ${JPEG_LIBRARY}
    ${LIBPNG_LIBRARIES}
    ${ZLIB_LIBRARIES}
    ${DL_LIBRARY}
    ${M_LIBRARY}
    PkgConfig::XFT
    Threads::Threads
)

target_link_libraries(ntk_static
    ${JPEG_LIBRARY}
    ${LIBPNG_LIBRARIES}
    ${ZLIB_LIBRARIES}
    ${DL_LIBRARY}
    ${M_LIBRARY}
    PkgConfig::XFT
    Threads::Threads
)

# --------------------------------------------------
# ntk_gl (optional)
# --------------------------------------------------

if(ENABLE_GL)

    set(NTK_GL_SOURCES
        gl_draw.cxx
        gl_start.cxx
        glut_compatability.cxx
        glut_font.cxx
        Fl_Gl_Choice.cxx
        Fl_Gl_Device_Plugin.cxx
        Fl_Gl_Overlay.cxx
        Fl_Gl_Window.cxx
    )

    add_library(ntk_gl SHARED ${NTK_GL_SOURCES})

    target_link_libraries(ntk_gl
        ${GL_LIBRARIES}
        ${X11_LIBRARIES}
        ${DL_LIBRARY}
        ${M_LIBRARY}
        Threads::Threads
    )

    add_library(ntk_gl_static STATIC ${NTK_GL_SOURCES})

    target_link_libraries(ntk_gl_static
        ${GL_LIBRARIES}
        ${X11_LIBRARIES}
        ${DL_LIBRARY}
        ${M_LIBRARY}
        Threads::Threads
    )
endif()

# --------------------------------------------------
# ntk-chtheme program
# --------------------------------------------------

add_executable(ntk-chtheme ntk-chtheme.cxx)
target_link_libraries(ntk-chtheme ntk ntk_images)

