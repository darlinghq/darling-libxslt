project(libxslt)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_definitions(-D__APPLE__ -D__MACH__)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-DHAVE_STDINT_H=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
add_definitions(-D__ENVIRONMENT_MAC_OS_X_VERSION_MIN_REQUIRED__=1080)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/darling")
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE) 
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../icu/icuSources/common)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../libxml2/include)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/../libxml2/include)

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

add_library(xslt SHARED ${xslt_SRCS})
target_link_libraries(xslt system xml2)

add_library(exslt SHARED ${exslt_SRCS})
target_link_libraries(exslt system xslt)

install(TARGETS xslt exslt DESTINATION ${CMAKE_INSTALL_LIBDIR}/darling)

