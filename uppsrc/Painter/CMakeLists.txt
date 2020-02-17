# CMakeLists.txt generated Fri Feb 14 11:53:31 +05 2020
cmake_minimum_required ( VERSION 2.8.10 )

#################################################
# In-Source builds are strictly prohibited.
#################################################
if ( ${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR} )
  message ( FATAL_ERROR
  "\n****************************** ERROR ******************************\n"
  "In-source build are not allowed. "
  "Please do not polute the sources with binaries or any project unrelated files. "
  "To remove generated files run:\n"
  "'rm -rf CMakeCache.txt CMakeFiles'\n"
  "To build the project, please do the following:\n"
  "'mkdir build && cd build && cmake ..'"
  "\n****************************** ERROR ******************************\n")
endif()

# Set the default library directory to store built libraries
set ( LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib )

# Precompiled headers file
set ( PCH_FILE ${CMAKE_CURRENT_SOURCE_DIR}/Painter.h )

list ( APPEND HEADER_LIST
      Painter.h
      Painter.hpp
      Painting.h
      LinearPath.h
      Interpolator.hpp
      Fillers.h
      BufferPainter.h
      SvgInternal.h
)

list ( APPEND SOURCE_LIST_CPP
      Painter.cpp
      SvgArc.cpp
      PainterPath.cpp
      DrawOp.cpp
      Painting.cpp
      PaintPainting.cpp
      Xform2D.cpp
      Approximate.cpp
      Stroker.cpp
      Dasher.cpp
      Transformer.cpp
      Rasterizer.cpp
      RasterizerClip.cpp
      RenderChar.cpp
      Fillers.cpp
      Context.cpp
      Path.cpp
      Render.cpp
      Image.cpp
      Mask.cpp
      Gradient.cpp
      RadialGradient.cpp
      OnPath.cpp
      SvgUtil.cpp
      SvgBounds.cpp
      SvgStyle.cpp
      SvgParser.cpp
)

list ( APPEND SOURCE_LIST_ICPP
      PainterInit.icpp
)

list ( APPEND Painter_DEPEND_LIST
      Draw-lib
)

# Module properties
create_cpps_from_icpps()
set_source_files_properties ( ${HEADER_LIST} PROPERTIES HEADER_FILE_ONLY ON )
add_library ( Painter-lib ${LIB_TYPE} ${SOURCE_LIST_CPP} ${SOURCE_LIST_C})
target_include_directories ( Painter-lib PUBLIC ${INCLUDE_LIST} )
set_property ( TARGET Painter-lib APPEND PROPERTY COMPILE_OPTIONS "${COMPILE_FLAGS_LIST}" )

# Module link
if ( Painter_DEPEND_LIST OR LINK_LIST )
  target_link_libraries ( Painter-lib ${Painter_DEPEND_LIST} ${LINK_LIST} )
endif()

# Precompiled headers settings
get_directory_property ( PCH_COMPILE_DEFINITIONS COMPILE_DEFINITIONS )
set_target_properties ( Painter-lib PROPERTIES COMPILE_FLAGS_LIST "${COMPILE_FLAGS_LIST}" )
set_target_properties ( Painter-lib PROPERTIES PCH_FILE "${PCH_FILE}" )
set_target_properties ( Painter-lib PROPERTIES PCH_INCLUDE_LIST "${INCLUDE_LIST}" )
set_target_properties ( Painter-lib PROPERTIES PCH_COMPILE_DEFINITIONS "${PCH_COMPILE_DEFINITIONS}" )

list ( LENGTH PCH_FILE PCH_FILE_LENGTH )
if ( PCH_FILE_LENGTH GREATER 1 )
  message ( FATAL_ERROR "Precompiled headers list can contain only one header file!" )
endif()
if ( PCH_FILE AND DEFINED flagPCH )
  get_filename_component ( PCH_NAME ${PCH_FILE} NAME )
  set ( PCH_DIR ${PROJECT_PCH_DIR}/Painter-lib )
  set ( PCH_HEADER ${PCH_DIR}/${PCH_NAME} )
  if ( ${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU" )
      if ( ${CMAKE_VERBOSE_MAKEFILE} EQUAL 1 )
        set ( PCH_INCLUDE_PARAMS " -H -Winvalid-pch -include ${PCH_HEADER}" )
      else()
        set ( PCH_INCLUDE_PARAMS " -Winvalid-pch -include ${PCH_HEADER}" )
      endif()
  endif()
  if ( ${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang" )
      set ( PCH_INCLUDE_PARAMS " -Winvalid-pch -include-pch ${PCH_HEADER}.pch" )
  endif()
  if ( MSVC )
      set_target_properties ( Painter-lib PROPERTIES COMPILE_FLAGS "-Yu${PCH_NAME} -Fp${PCH_HEADER}.pch" )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "Yc${PCH_NAME} -Fp${PCH_HEADER}.pch" )
  endif()
  if ( PCH_INCLUDE_PARAMS )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE_PARAMS}" )
  endif()
endif()

