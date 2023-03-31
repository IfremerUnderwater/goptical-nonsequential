project(goptical CXX C)

set(GOPTICAL_VERSION "1.0")

cmake_minimum_required(VERSION 2.8)
set (CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

macro(SUBDIRLIST result curdir)
  file(GLOB children RELATIVE ${curdir} ${curdir}/*)
  set(dirlist "")
  foreach(child ${children})
    if(IS_DIRECTORY ${curdir}/${child})
        LIST(APPEND dirlist ${child})
    endif()
  endforeach()
  set(${result} ${dirlist})
endmacro()

find_package(Git)
include_directories(include)

# install includes
install(
  DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/
  DESTINATION include
  COMPONENT headers
)

# ==============================================================================
# INSTALL RULES
# ==============================================================================

install(EXPORT goptical-targets DESTINATION lib)
install(EXPORT goptical-targets
  FILE gopticalTargets.cmake
  NAMESPACE goptical::
  DESTINATION share/goptical/cmake
)


# Create a gopticalConfig.cmake file. <name>Config.cmake files are searched by
# find_package() automatically. We configure that file so that we can put any
# information we want in it, e.g. version numbers, include directories, etc.
configure_file("${CMAKE_SOURCE_DIR}/cmake/gopticalConfig.cmake.in"
               "${CMAKE_CURRENT_BINARY_DIR}/gopticalConfig.cmake" @ONLY)

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/gopticalConfig.cmake"
        DESTINATION share/goptical/cmake)

add_subdirectory(src)
add_subdirectory(examples)