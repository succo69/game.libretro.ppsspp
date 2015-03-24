project(game.libretro.ppsspp)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)

include(ExternalProject)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  set(LIBRETRO_DEBUG DEBUG=1)
endif()

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  set(BUILD_COMMAND mingw32-make -C libretro platform=win ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND make -C libretro ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin")
  set(BUILD_COMMAND make -C libretro platform=osx ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
# TODO
endif()

externalproject_add(ppsspp
                    GIT_REPOSITORY https://github.com/libretro/libretro-ppsspp
                    GIT_TAG 9670100
                    PREFIX ppsspp
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

set(PPSSPP_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/ppsspp/src/ppsspp/libretro/ppsspp_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                         game.libretro.ppsspp${CMAKE_SHARED_LIBRARY_SUFFIX} ppsspp)

build_addon(game.libretro.ppsspp PPSSPP DEPLIBS)
add_dependencies(game.libretro.ppsspp ppsspp)

include(CPack)
