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

set(DOCDIR "${CMAKE_INSTALL_FULL_DATADIR}/doc/${PROJECT_NAME}")

# ----------------------------------------------------------
# FLTK_DOCDIR (mirror waf)
# ----------------------------------------------------------

set(FLTK_DOCDIR "${CMAKE_INSTALL_FULL_DOCDIR}")

# ----------------------------------------------------------
# Version
# ----------------------------------------------------------

set(VERSION "${PROJECT_VERSION}")

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

#pkg_check_modules(X11 REQUIRED IMPORTED_TARGET x11)
#pkg_check_modules(FONTCONFIG REQUIRED IMPORTED_TARGET fontconfig)
#pkg_check_modules(XFT REQUIRED IMPORTED_TARGET xft)
#pkg_check_modules(CAIRO REQUIRED IMPORTED_TARGET cairo>=1.10.0)
#pkg_check_modules(LIBPNG REQUIRED IMPORTED_TARGET libpng)
#pkg_check_modules(ZLIB REQUIRED IMPORTED_TARGET zlib)

pkg_check_modules(X11 REQUIRED x11)
pkg_check_modules(FONTCONFIG REQUIRED fontconfig)
#pkg_check_modules(XFT REQUIRED xft)
pkg_check_modules(XFT REQUIRED IMPORTED_TARGET xft)
pkg_check_modules(CAIRO REQUIRED cairo>=1.10.0)
pkg_check_modules(LIBPNG REQUIRED libpng)
pkg_check_modules(ZLIB REQUIRED zlib)

if(FONTCONFIG_FOUND)
    set(HAVE_FONTCONFIG 1)
else()
    set(HAVE_FONTCONFIG 0)
endif()

if(XFT_FOUND)
    set(HAVE_XFT 1)
else()
    set(HAVE_XFT 0)
endif()

if(CAIRO_FOUND)
    set(HAVE_CAIRO 1)
else()
    set(HAVE_CAIRO 0)
endif()

if(LIBPNG_FOUND)
    set(HAVE_LIBPNG 1)
else()
    set(HAVE_LIBPNG 0)
endif()

if(ZLIB_FOUND)
    set(HAVE_LIBZ 1)
else()
    set(HAVE_LIBZ 0)
endif()

find_library(JPEG_LIBRARY jpeg REQUIRED)
find_package(Threads REQUIRED)
find_library(DL_LIBRARY dl REQUIRED)
find_library(M_LIBRARY m REQUIRED)

if(CMAKE_HAVE_LIBC_PTHREAD)
    set(HAVE_PTHREAD 1)
else()
    set(HAVE_PTHREAD 0)
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
check_include_file(sys/stdtypes.h HAVE_SYS_STDTYPES_H)
check_include_file(png.h HAVE_PNG_H)

check_function_exists(strtoll HAVE_STRTOLL)
check_function_exists(scandir HAVE_SCANDIR)
check_function_exists(strcasecmp HAVE_STRCASECMP)
check_function_exists(snprintf HAVE_SNPRINTF)
check_function_exists(vsnprintf HAVE_VSNPRINTF)

check_function_exists(vsnprintf HAVE_VSNPRINTF)
if(HAVE_VSNPRINTF)
    set(HAVE_SNPRINTF 1)
endif()

# pthread recursive mutex
check_c_source_compiles(
"
#include <pthread.h>
int main(int argc, const char **argv)
{
    return PTHREAD_MUTEX_RECURSIVE;
}
"
HAVE_PTHREAD_MUTEX_RECURSIVE
)


# long long existence
check_c_source_compiles("
#include <stdio.h>
int main() { long long x = 0; return (int)x; }
" HAVE_LONG_LONG)

# ----------------------------------------------------------
# Libraries
# ----------------------------------------------------------

check_library_exists(jpeg jpeg_CreateCompress "" HAVE_LIBJPEG)

# ----------------------------------------------------------
# GL Support (optional)
# ----------------------------------------------------------

if(ENABLE_GL)
    pkg_check_modules(GL REQUIRED glu)
    set(HAVE_GL 1)
    check_include_file(GL/glu.h HAVE_GL_GLU_H)
endif()

# ----------------------------------------------------------
# Platform Logic (mirror waf)
# ----------------------------------------------------------

if(APPLE)
    add_definitions(-D__APPLE__)
elseif(WIN32)
    add_definitions(-DWIN32)
else()
    set(HAVE_X11 1)
    set(USE_X11 1)
    set(USE_XFT 1)
endif()

set(USE_POLL 1)
set(BORDER_WIDTH 1)
set(HAVE_SCANDIR_POSIX 1)
set(HAVE_DLSYM 1)

# ----------------------------------------------------------
# Size Checks
# ----------------------------------------------------------

check_type_size("short" SIZEOF_SHORT)
check_type_size("int" SIZEOF_INT)
check_type_size("long" SIZEOF_LONG)

# U16 / U32 / U64 logic (exact Waf logic)
if(SIZEOF_SHORT EQUAL 2)
    set(U16 "unsigned short")
endif()

if(SIZEOF_INT EQUAL 4)
    set(U32 "unsigned int")
elseif(SIZEOF_LONG EQUAL 4)
    set(U32 "unsigned long")
endif()

if(SIZEOF_INT EQUAL 8)
    set(U64 "unsigned int")
elseif(SIZEOF_LONG EQUAL 8)
    set(U64 "unsigned long")
endif()

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

add_subdirectory(src)
add_subdirectory(fluid)
if (ENABLE_TEST)
    add_subdirectory(test)
endif()

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

configure_file(${CMAKE_SOURCE_DIR}/cmake/ntk.pc.in ntk.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/ntk.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

configure_file(
    ${CMAKE_SOURCE_DIR}/cmake/ntk_images.pc.in
    ${CMAKE_BINARY_DIR}/ntk_images.pc
    @ONLY
)

install(FILES
    ${CMAKE_BINARY_DIR}/ntk_images.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

if(ENABLE_GL)
    configure_file(
        ${CMAKE_SOURCE_DIR}/cmake/ntk_gl.pc.in
        ${CMAKE_BINARY_DIR}/ntk_gl.pc
        @ONLY
    )

    install(FILES
        ${CMAKE_BINARY_DIR}/ntk_gl.pc
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
    )
endif()

install(TARGETS
    ntk ntk_static
    ntk_images ntk_images_static
    ntk-chtheme
    ntk-fluid
    EXPORT ntkTargets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

# --------------------------------------------------
# uninstall target
# --------------------------------------------------

if(NOT TARGET uninstall)
    configure_file(
        "${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
        "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
        IMMEDIATE @ONLY)

    add_custom_target(uninstall
        COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
endif()