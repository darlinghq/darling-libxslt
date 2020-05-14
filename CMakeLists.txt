project(libxslt)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_definitions(-DHAVE_STDINT_H=1)
add_definitions(-D__ENVIRONMENT_MAC_OS_X_VERSION_MIN_REQUIRED__=1080)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w")

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../icu/icuSources/common)

set(xslt_SRCS
	libxslt/attributes.c
	libxslt/attrvt.c
	libxslt/documents.c
	libxslt/extensions.c
	libxslt/extra.c
	libxslt/functions.c
	libxslt/imports.c
	libxslt/keys.c
	libxslt/namespaces.c
	libxslt/numbers.c
	libxslt/pattern.c
	libxslt/preproc.c
	libxslt/security.c
	libxslt/templates.c
	libxslt/transform.c
	libxslt/variables.c
	libxslt/xslt.c
	libxslt/xsltlocale.c
	libxslt/xsltutils.c
)

set(exslt_SRCS
	libexslt/common.c
	libexslt/crypto.c
	libexslt/date.c
	libexslt/dynamic.c
	libexslt/exslt.c
	libexslt/functions.c
	libexslt/math.c
	libexslt/saxon.c
	libexslt/sets.c
	libexslt/strings.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/libxslt.1.dylib")
set(DYLIB_COMPAT_VERSION "3.0.0")
set(DYLIB_CURRENT_VERSION "3.0.0")
add_darling_library(xslt SHARED ${xslt_SRCS})
make_fat(xslt)
target_link_libraries(xslt system xml2)
set_target_properties(xslt PROPERTIES OUTPUT_NAME "xslt.1")
InstallSymlink("libxslt.1.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libxslt.dylib")

set(DYLIB_INSTALL_NAME "/usr/lib/libexslt.0.dylib")
set(DYLIB_COMPAT_VERSION "9.0.0")
set(DYLIB_CURRNT_VERSION "9.15.0")
add_darling_library(exslt SHARED ${exslt_SRCS})
make_fat(exslt)

target_link_libraries(exslt system xslt)
set_target_properties(exslt PROPERTIES OUTPUT_NAME "exslt.0")
InstallSymlink("libexslt.0.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libexslt.dylib")

install(TARGETS xslt exslt DESTINATION libexec/darling/usr/lib)

install(FILES doc/xsltproc.1 DESTINATION libexec/darling/usr/share/man/man1)
install(FILES libexslt/libexslt.3 libxslt/libxslt.3 DESTINATION libexec/darling/usr/share/man/man3)
