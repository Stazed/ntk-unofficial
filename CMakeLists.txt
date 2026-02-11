cmake_minimum_required(VERSION 3.16)
project(ntk VERSION 1.3.1002 LANGUAGES C CXX)

include(GNUInstallDirs)
include(CheckIncludeFile)
include(CheckFunctionExists)
include(CheckSymbolExists)
include(CheckTypeSize)
include(CheckCSourceCompiles)
include(CheckCXXSourceCompiles)
include(CheckLibraryExists)
include(CheckStructHasMember)

option(ENABLE_GL "Build with OpenGL support" OFF)
option(ENABLE_TEST "Build test programs" OFF)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# --------------------------------------------------
# Compiler flags
# --------------------------------------------------

add_compile_options(
    -Wall
    -pthread
    -D_LARGEFILE64_SOURCE
    -D_FILE_OFFSET_BITS=64
)

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_options(-g -O0)
else()
    add_compile_options(-O3 -fomit-frame-pointer -ffast-math -pipe)
    add_compile_definitions(NDEBUG)
endif()

# --------------------------------------------------
# Dependencies via pkg-config
# --------------------------------------------------

find_package(PkgConfig REQUIRED)

#pkg_check_modules(X11 REQUIRED x11)
#pkg_check_modules(FONTCONFIG REQUIRED fontconfig)
#pkg_check_modules(XFT REQUIRED xft)
#pkg_check_modules(CAIRO REQUIRED cairo>=1.10.0)
#pkg_check_modules(LIBPNG REQUIRED libpng)
#pkg_check_modules(ZLIB REQUIRED zlib)

pkg_check_modules(X11 REQUIRED IMPORTED_TARGET x11)
pkg_check_modules(FONTCONFIG REQUIRED IMPORTED_TARGET fontconfig)
pkg_check_modules(XFT REQUIRED IMPORTED_TARGET xft)
pkg_check_modules(CAIRO REQUIRED IMPORTED_TARGET cairo>=1.10.0)
pkg_check_modules(LIBPNG REQUIRED IMPORTED_TARGET libpng)
pkg_check_modules(ZLIB REQUIRED IMPORTED_TARGET zlib)


find_library(JPEG_LIBRARY jpeg REQUIRED)
find_package(Threads REQUIRED)
find_library(DL_LIBRARY dl REQUIRED)
find_library(M_LIBRARY m REQUIRED)

if(ENABLE_GL)
    pkg_check_modules(GL REQUIRED glu)
    add_compile_definitions(HAVE_GL=1)
endif()

# --------------------------------------------------
# Header checks
# --------------------------------------------------

check_include_file(unistd.h HAVE_UNISTD_H)
check_include_file(pthread.h HAVE_PTHREAD_H)
check_include_file(dirent.h HAVE_DIRENT_H)
check_include_file(string.h HAVE_STRINGS_H)
check_include_file(locale.h HAVE_LOCALE_H)
check_include_file(sys/select.h HAVE_SYS_SELECT_H)
check_include_file(dlfcn.h HAVE_DLFCN_H)
check_include_file(png.h HAVE_PNG_H)

check_function_exists(strtoll HAVE_STRTOLL)
check_function_exists(scandir HAVE_SCANDIR)
check_function_exists(strcasecmp HAVE_STRCASECMP)
check_function_exists(snprintf HAVE_SNPRINTF)
check_function_exists(vsnprintf HAVE_VSNPRINTF)

check_type_size(short SIZEOF_SHORT)
check_type_size(int SIZEOF_INT)
check_type_size(long SIZEOF_LONG)

# --------------------------------------------------
# Configure config.h
# --------------------------------------------------

include(TestBigEndian)
test_big_endian(IS_BIG_ENDIAN)

if(IS_BIG_ENDIAN)
    set(WORDS_BIGENDIAN 1)
else()
    set(WORDS_BIGENDIAN 0)
endif()

configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/config.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/config.h
)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/src/xutf8/headers
    ${CMAKE_CURRENT_BINARY_DIR}
)

# --------------------------------------------------
# NTK Core Library
# --------------------------------------------------

set(NTK_SOURCES
src/Fl_Cairo_Graphics_Driver.cxx
src/Fl.cxx
src/Fl_Adjuster.cxx
src/Fl_Bitmap.cxx
src/Fl_Browser.cxx
src/Fl_Browser_.cxx
src/Fl_Browser_load.cxx
src/Fl_Box.cxx
src/Fl_Button.cxx
src/Fl_Chart.cxx
src/Fl_Check_Browser.cxx
src/Fl_Check_Button.cxx
src/Fl_Choice.cxx
src/Fl_Color_Chooser.cxx
src/Fl_Counter.cxx
src/Fl_Dial.cxx
src/Fl_Dial_Base.cxx
src/Fl_Device.cxx
src/Fl_Double_Window.cxx
src/Fl_File_Browser.cxx
src/Fl_File_Chooser.cxx
src/Fl_File_Chooser2.cxx
src/Fl_File_Icon.cxx
src/Fl_File_Input.cxx
src/Fl_Group.cxx
src/Fl_Help_View.cxx
src/Fl_Image.cxx
src/Fl_Input.cxx
src/Fl_Input_.cxx
src/Fl_Light_Button.cxx
src/Fl_Menu.cxx
src/Fl_Menu_.cxx
src/Fl_Menu_Bar.cxx
src/Fl_Sys_Menu_Bar.cxx
src/Fl_Menu_Button.cxx
src/Fl_Menu_Window.cxx
src/Fl_Menu_add.cxx
src/Fl_Menu_global.cxx
src/Fl_Multi_Label.cxx
src/Fl_Native_File_Chooser.cxx
src/Fl_Overlay_Window.cxx
src/Fl_Pack.cxx
src/Fl_Paged_Device.cxx
src/Fl_Panzoomer.cxx
src/Fl_Pixmap.cxx
src/Fl_Positioner.cxx
src/Fl_Preferences.cxx
src/Fl_Printer.cxx
src/Fl_Progress.cxx
src/Fl_Repeat_Button.cxx
src/Fl_Return_Button.cxx
src/Fl_Round_Button.cxx
src/Fl_Scroll.cxx
src/Fl_Scrollbar.cxx
src/Fl_Shared_Image.cxx
src/Fl_Single_Window.cxx
src/Fl_Slider.cxx
src/Fl_Table.cxx
src/Fl_Table_Row.cxx
src/Fl_Tabs.cxx
src/Fl_Text_Buffer.cxx
src/Fl_Text_Display.cxx
src/Fl_Text_Editor.cxx
src/Fl_Tile.cxx
src/Fl_Tiled_Image.cxx
src/Fl_Tree.cxx
src/Fl_Tree_Item.cxx
src/Fl_Tree_Item_Array.cxx
src/Fl_Tree_Prefs.cxx
src/Fl_Tooltip.cxx
src/Fl_Valuator.cxx
src/Fl_Value_Input.cxx
src/Fl_Value_Output.cxx
src/Fl_Value_Slider.cxx
src/Fl_Widget.cxx
src/Fl_Window.cxx
src/Fl_Window_fullscreen.cxx
src/Fl_Window_hotspot.cxx
src/Fl_Window_iconize.cxx
src/Fl_Wizard.cxx
src/Fl_XBM_Image.cxx
src/Fl_XPM_Image.cxx
src/Fl_abort.cxx
src/Fl_add_idle.cxx
src/Fl_arg.cxx
src/Fl_compose.cxx
src/Fl_display.cxx
src/Fl_get_key.cxx
src/Fl_get_system_colors.cxx
src/Fl_grab.cxx
src/Fl_lock.cxx
src/Fl_own_colormap.cxx
src/Fl_visual.cxx
src/Fl_x.cxx
src/filename_absolute.cxx
src/filename_expand.cxx
src/filename_ext.cxx
src/filename_isdir.cxx
src/filename_list.cxx
src/filename_match.cxx
src/filename_setext.cxx
src/fl_arc.cxx
src/fl_arci.cxx
src/fl_ask.cxx
src/fl_boxtype.cxx
src/fl_color.cxx
src/fl_cursor.cxx
src/fl_curve.cxx
src/fl_diamond_box.cxx
src/fl_dnd.cxx
src/fl_draw.cxx
src/Fl_Cairo.cxx
src/fl_draw_image.cxx
src/fl_draw_pixmap.cxx
src/fl_encoding_latin1.cxx
src/fl_encoding_mac_roman.cxx
src/fl_engraved_label.cxx
src/fl_file_dir.cxx
src/fl_font.cxx
src/fl_labeltype.cxx
src/fl_line_style.cxx
src/fl_open_uri.cxx
src/fl_oval_box.cxx
src/fl_overlay.cxx
src/fl_read_image.cxx
src/fl_rect.cxx
src/fl_round_box.cxx
src/fl_rounded_box.cxx
src/fl_set_font.cxx
src/fl_set_fonts.cxx
src/fl_scroll_area.cxx
src/fl_shadow_box.cxx
src/fl_shortcut.cxx
src/fl_show_colormap.cxx
src/fl_symbols.cxx
src/fl_vertex.cxx
src/screen_xywh.cxx
src/fl_utf8.cxx
src/Fl_Theme.cxx
src/Fl_Theme_Chooser.cxx
src/Cairo_Theme.cxx
src/Gleam_Theme.cxx
src/GTK_Theme.cxx
src/Clean_Theme.cxx
src/Crystal_Theme.cxx
src/Vector_Theme.cxx
src/Vector2_Theme.cxx
src/themes.cxx
src/ps_image.cxx
src/fl_utf.c
src/vsnprintf.c
src/xutf8/case.c
src/xutf8/is_right2left.c
src/xutf8/is_spacing.c
src/xutf8/keysym2Ucs.c
src/xutf8/utf8Input.c
src/xutf8/utf8Utils.c
src/xutf8/utf8Wrap.c
src/numericsort.c
src/flstring.c
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
    src/fl_images_core.cxx
    src/Fl_BMP_Image.cxx
    src/Fl_File_Icon2.cxx
    src/Fl_GIF_Image.cxx
    src/Fl_Help_Dialog.cxx
    src/Fl_JPEG_Image.cxx
    src/Fl_PNG_Image.cxx
    src/Fl_PNM_Image.cxx
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
        src/gl_draw.cxx
        src/gl_start.cxx
        src/glut_compatability.cxx
        src/glut_font.cxx
        src/Fl_Gl_Choice.cxx
        src/Fl_Gl_Device_Plugin.cxx
        src/Fl_Gl_Overlay.cxx
        src/Fl_Gl_Window.cxx
    )

    add_library(ntk_gl SHARED ${NTK_GL_SOURCES})

    target_link_libraries(ntk_gl
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

add_executable(ntk-chtheme src/ntk-chtheme.cxx)
target_link_libraries(ntk-chtheme ntk ntk_images)

install(TARGETS
    ntk ntk_static
    ntk_images ntk_images_static
    ntk-chtheme
    EXPORT ntkTargets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

# --------------------------------------------------
# Install headers
# --------------------------------------------------

install(DIRECTORY FL/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/ntk/FL
    FILES_MATCHING PATTERN "*.H" PATTERN "*.h"
)

# --------------------------------------------------
# Pkg-config files
# --------------------------------------------------

configure_file(ntk.pc.in ntk.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/ntk.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)
