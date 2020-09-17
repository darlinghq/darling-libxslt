project(libxslt)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_definitions(
	-DHAVE_CONFIG_H
	-D_REENTRANT
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w")

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/libxslt
	"${CMAKE_CURRENT_SOURCE_DIR}/Pregenerated Files/include"
)

set(xslt_SRCS
	libxslt/libxslt/attributes.c
	libxslt/libxslt/attrvt.c
	libxslt/libxslt/documents.c
	libxslt/libxslt/extensions.c
	libxslt/libxslt/extra.c
	libxslt/libxslt/functions.c
	libxslt/libxslt/imports.c
	libxslt/libxslt/keys.c
	libxslt/libxslt/namespaces.c
	libxslt/libxslt/numbers.c
	libxslt/libxslt/pattern.c
	libxslt/libxslt/preproc.c
	libxslt/libxslt/security.c
	libxslt/libxslt/templates.c
	libxslt/libxslt/transform.c
	libxslt/libxslt/variables.c
	libxslt/libxslt/xslt.c
	libxslt/libxslt/xsltlocale.c
	libxslt/libxslt/xsltutils.c
)

set(exslt_SRCS
	libxslt/libexslt/common.c
	libxslt/libexslt/crypto.c
	libxslt/libexslt/date.c
	libxslt/libexslt/dynamic.c
	libxslt/libexslt/exslt.c
	libxslt/libexslt/functions.c
	libxslt/libexslt/math.c
	libxslt/libexslt/saxon.c
	libxslt/libexslt/sets.c
	libxslt/libexslt/strings.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/libxslt.1.dylib")
set(DYLIB_COMPAT_VERSION "3.0.0")
set(DYLIB_CURRENT_VERSION "3.26.0")
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

install(FILES libxslt/doc/xsltproc.1 DESTINATION libexec/darling/usr/share/man/man1)
install(FILES libxslt/libexslt/libexslt.3 libxslt/libxslt/libxslt.3 DESTINATION libexec/darling/usr/share/man/man3)
