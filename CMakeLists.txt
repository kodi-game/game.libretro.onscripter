cmake_minimum_required(VERSION 3.5)
project(game.libretro.onscripter)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

find_package(Kodi REQUIRED)

# CMake on windows only searches for .lib libraries (static library or import library).
# The libretro game library is dynamically loaded so even if an import library would exists it's of no use.
if(CMAKE_SYSTEM_NAME STREQUAL Windows)
  set(_CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_FIND_LIBRARY_SUFFIXES})
  set(CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_SHARED_LIBRARY_SUFFIX})
endif()
find_library(ONSCRIPTER_LIB NAMES onscripter_libretro${CMAKE_SHARED_LIBRARY_SUFFIX} PATH_SUFFIXES libretro)
if(CMAKE_SYSTEM_NAME STREQUAL Windows)
  set(CMAKE_FIND_LIBRARY_SUFFIXES ${_CMAKE_FIND_LIBRARY_SUFFIXES})
endif()

set(ONSCRIPTER_CUSTOM_BINARY ${ONSCRIPTER_LIB}
    ${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX})

build_addon(${PROJECT_NAME} ONSCRIPTER "")

include(CPack)
