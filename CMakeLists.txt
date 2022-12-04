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

fips_begin_lib(libmodplug)
    fips_src(libmodplug/src GROUP .)
fips_end_lib()
target_include_directories(libmodplug PRIVATE libmodplug/src/libmodplug)
if (FIPS_WINDOWS)
    target_compile_definitions(libmodplug PRIVATE HAVE_SINF=1 HAVE_STDINT_H=1)
else()
    target_compile_definitions(libmodplug PRIVATE HAVE_SINF=1 HAVE_SETENV=1 HAVE_STDINT_H=1)
endif()
target_compile_definitions(libmodplug PUBLIC MODPLUG_STATIC=1)

if (FIPS_CLANG)
    target_compile_options(libmodplug PRIVATE -Wno-deprecated-declarations -Wno-unknown-warning-option -Wno-unused-but-set-variable -Wno-shorten-64-to-32 -Wno-unused-parameter -Wno-unused-function -Wno-unused-private-field -Wno-dangling-else -Wno-missing-braces -Wno-unused-value -Wno-parentheses -Wno-sign-compare -Wno-unused-variable -Wno-deprecated-register -Wno-writable-strings -Wno-reorder -Wno-delete-non-virtual-dtor -Wno-deprecated-register -Wno-missing-braces -Wno-unused-function -Wno-unused-variable)
elseif (FIPS_GCC)
    target_compile_options(libmodplug PRIVATE args -Wno-stringop-truncation -Wno-unused-parameter -Wno-restrict -Wno-format-overflow -Wno-implicit-fallthrough -Wno-misleading-indentation -Wno-unused-function -Wno-missing-braces -Wno-unused-value -Wno-parentheses -Wno-sign-compare -Wno-unused-variable -Wno-reorder -Wno-delete-non-virtual-dtor -Wno-maybe-uninitialized -Wno-unused-but-set-variable -Wno-unused-result)
elseif (FIPS_MSVC)
    target_compile_options(libmodplug PRIVATE args /wd4819 /wd4146 /wd4244 /wd4996 /wd4018 /wd4267 /wd4018)
endif()
