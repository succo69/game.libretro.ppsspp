project(game.libretro.ppsspp)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)
find_package(libretro-ppsspp REQUIRED)

set(DEPLIBS ppsspp)
set(PPSSPP_CUSTOM_BINARY ${PPSSPP_LIB}
                       ${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX} ppsspp)

build_addon(${PROJECT_NAME} PPSSPP DEPLIBS)
