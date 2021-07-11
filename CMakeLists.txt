project( implot CXX )
cmake_minimum_required( VERSION 3.20 )

set( CMAKE_CXX_STANDARD_REQUIRED TRUE )
set( CMAKE_CXX_STANDARD 17 )
set( CMAKE_POSITION_INDEPENDENT_CODE TRUE )

set(
  INSTALL_TARGETS_DEFAULT_ARGS
  EXPORT implotTargets
  ARCHIVE DESTINATION  "lib/"
  LIBRARY DESTINATION  "lib/"
  RUNTIME DESTINATION  "bin/"
  INCLUDES DESTINATION "include/")

find_package( imgui REQUIRED )

add_library(
  implot
  STATIC
  implot.cpp
  implot_items.cpp
  )

target_link_libraries(
  implot
  PUBLIC
  imgui::base
  )

target_include_directories(
  implot
  PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:include/>
  )

install(
  TARGETS implot
  ${INSTALL_TARGETS_DEFAULT_ARGS})

install(
  FILES
  implot.h
  DESTINATION
  include/)

option(
  IMPLOT_BUILD_DEMO
  "Build the demo library"
  FALSE )

if( IMPLOT_BUILD_DEMO )
  add_library(
    demo
    STATIC
    implot_demo.cpp
    )
  set_target_properties(
    demo
    PROPERTIES
    OUTPUT_NAME implot-demo )
  target_link_libraries(
    demo
    PUBLIC
    implot
    )
  install(
    TARGETS demo
    ${INSTALL_TARGETS_DEFAULT_ARGS}
    )
endif()

include( CMakePackageConfigHelpers )
configure_file( implotConfig.cmake.in implotConfig.cmake @ONLY )

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/implotConfig.cmake"
  DESTINATION lib/cmake/implot)

install(
  EXPORT implotTargets
  NAMESPACE implot::
  DESTINATION lib/cmake/implot/ )
