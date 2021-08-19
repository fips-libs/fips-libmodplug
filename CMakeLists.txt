#
# project: fips-libmodplug
#
if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 3.0)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup()
    fips_project(fips-libmodplug)
endif()

fips_include_directories(libmodplug/src/libmodplug)
fips_begin_module(libmodplug)
    fips_src(libmodplug/src GROUP .)
fips_end_module()
if (FIPS_WINDOWS)
    target_compile_definitions(libmodplug PRIVATE HAVE_SINF=1 HAVE_STDINT_H=1)
else() 
    target_compile_definitions(libmodplug PRIVATE HAVE_SINF=1 HAVE_SETENV=1 HAVE_STDINT_H=1)
endif()
target_compile_definitions(libmodplug PUBLIC MODPLUG_STATIC=1)

set(args)
if (FIPS_CLANG)
    set(args -Wno-unknown-warning-option -Wno-unused-but-set-variable -Wno-shorten-64-to-32 -Wno-unused-parameter -Wno-unused-function -Wno-unused-private-field -Wno-dangling-else -Wno-missing-braces -Wno-unused-value -Wno-parentheses -Wno-sign-compare -Wno-unused-variable -Wno-deprecated-register -Wno-writable-strings -Wno-reorder -Wno-delete-non-virtual-dtor -Wno-deprecated-register -Wno-missing-braces -Wno-unused-function -Wno-unused-variable)
elseif (FIPS_GCC)
    set(args -Wno-stringop-truncation -Wno-unused-parameter -Wno-restrict -Wno-format-overflow -Wno-implicit-fallthrough -Wno-misleading-indentation -Wno-unused-function -Wno-missing-braces -Wno-unused-value -Wno-parentheses -Wno-sign-compare -Wno-unused-variable -Wno-reorder -Wno-delete-non-virtual-dtor -Wno-maybe-uninitialized -Wno-unused-but-set-variable -Wno-unused-result)
elseif (FIPS_WINDOWS)
    # 4814: he file contains a character that cannot be represented in the current code page...
    set(args /wd4819)
elseif (FIPS_UWP)
    # 4146: unary minus operator applied to unsigned type, result still unsigned
    # 4244: conversion from __int64 to int, possible loss of data
    set(args /wd4146 /wd4244)
endif()
if (args)
    target_compile_options(libmodplug PRIVATE ${args})
endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()

