cmake_minimum_required (VERSION 3.6.0)
project (psvpfstools)

set (CMAKE_CXX_STANDARD 17)

if (MSVC)
   add_definitions (/W3)
   add_definitions(-D_SCL_SECURE_NO_WARNINGS)
elseif (CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-unknown-pragmas -fPIC")
else()
   message (FATAL_ERROR "Unknown compiler")
   return()
endif()

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Build type is not specified. Set to Release.")
  set(CMAKE_BUILD_TYPE Release)
endif()

#additional search paths for cmake (in case of some custom dependencies)
set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/scripts/")

include(cmake/scripts/ConfigureDependencies.cmake)

SET(CMAKE_FIND_LIBRARY_SUFFIXES .a)

configure_json()
configureCLI11()
configure_zlib()
configure_libtomcrypt()

add_subdirectory(libb64 libraries/libb64)
add_subdirectory(libzRIF libraries/libzRIF)
add_subdirectory(psvpfsparser executables/psvpfsparser)
