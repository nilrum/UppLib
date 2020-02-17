# CMakeLists.txt generated Thu Feb 13 15:47:22 +05 2020
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

# options(BUILDER_OPTION) NOWARNINGS;
if ( CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_CLANG )
  set ( CMAKE_CXX_FLAGS_${CMAKE_BUILD_TYPE} "${CMAKE_CXX_FLAGS_${CMAKE_BUILD_TYPE}} -w")
  set ( CMAKE_C_FLAGS_${CMAKE_BUILD_TYPE} "${CMAKE_C_FLAGS_${CMAKE_BUILD_TYPE}} -w")
elseif ( MSVC )
  set ( CMAKE_CXX_FLAGS_${CMAKE_BUILD_TYPE} "${CMAKE_CXX_FLAGS_${CMAKE_BUILD_TYPE}} -W0")
  set ( CMAKE_C_FLAGS_${CMAKE_BUILD_TYPE} "${CMAKE_C_FLAGS_${CMAKE_BUILD_TYPE}} -W0")
endif()

list ( APPEND HEADER_LIST
      Pcre.h
      RegExp.h
      lib/config.h
)

list ( APPEND SOURCE_LIST_C
      lib.c
)

list ( APPEND SOURCE_LIST_CPP
      RegExp.cpp
)

list ( APPEND plugin_pcre_DEPEND_LIST
      Core-lib
)

# Module properties
create_cpps_from_icpps()
set_source_files_properties ( ${HEADER_LIST} PROPERTIES HEADER_FILE_ONLY ON )
add_library ( plugin_pcre-lib ${LIB_TYPE} ${SOURCE_LIST_CPP} ${SOURCE_LIST_C})
target_include_directories ( plugin_pcre-lib PUBLIC ${INCLUDE_LIST} )
set_property ( TARGET plugin_pcre-lib APPEND PROPERTY COMPILE_OPTIONS "${COMPILE_FLAGS_LIST}" )

# Module link
if ( plugin_pcre_DEPEND_LIST OR LINK_LIST )
  target_link_libraries ( plugin_pcre-lib ${plugin_pcre_DEPEND_LIST} ${LINK_LIST} )
endif()

# Precompiled headers settings
get_directory_property ( PCH_COMPILE_DEFINITIONS COMPILE_DEFINITIONS )
set_target_properties ( plugin_pcre-lib PROPERTIES COMPILE_FLAGS_LIST "${COMPILE_FLAGS_LIST}" )
set_target_properties ( plugin_pcre-lib PROPERTIES PCH_FILE "${PCH_FILE}" )
set_target_properties ( plugin_pcre-lib PROPERTIES PCH_INCLUDE_LIST "${INCLUDE_LIST}" )
set_target_properties ( plugin_pcre-lib PROPERTIES PCH_COMPILE_DEFINITIONS "${PCH_COMPILE_DEFINITIONS}" )

list ( LENGTH PCH_FILE PCH_FILE_LENGTH )
if ( PCH_FILE_LENGTH GREATER 1 )
  message ( FATAL_ERROR "Precompiled headers list can contain only one header file!" )
endif()
if ( PCH_FILE AND DEFINED flagPCH )
  get_filename_component ( PCH_NAME ${PCH_FILE} NAME )
  set ( PCH_DIR ${PROJECT_PCH_DIR}/plugin_pcre-lib )
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
      set_target_properties ( plugin_pcre-lib PROPERTIES COMPILE_FLAGS "-Yu${PCH_NAME} -Fp${PCH_HEADER}.pch" )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "Yc${PCH_NAME} -Fp${PCH_HEADER}.pch" )
  endif()
  if ( PCH_INCLUDE_PARAMS )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE_PARAMS}" )
  endif()
endif()

