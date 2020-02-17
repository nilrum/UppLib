# CMakeLists.txt generated Thu Feb 13 15:46:40 +05 2020
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

list ( APPEND HEADER_LIST
      zstd.h
      lib/zstd.h
)

list ( APPEND SOURCE_LIST_C
      lib/huf_decompress.c
      lib/zstd_ddict.c
      lib/zstd_decompress.c
      lib/zstd_decompress_block.c
      lib/entropy_common.c
      lib/fse_compress.c
      lib/fse_decompress.c
      lib/huf_compress.c
      lib/zstd_common.c
      lib/zstd_compress.c
      lib/zstd_opt.c
      lib/zstdmt_compress.c
      lib/zstd_lazy.c
      lib/zstd_ldm.c
      lib/zstd_fast.c
      lib/error_private.c
      lib/hist.c
      lib/pool.c
      lib/zstd_double_fast.c
)

list ( APPEND SOURCE_LIST_CPP
      Compress.cpp
      Decompress.cpp
      Util.cpp
)

list ( APPEND plugin_zstd_DEPEND_LIST
      Core-lib
)

# Module properties
create_cpps_from_icpps()
set_source_files_properties ( ${HEADER_LIST} PROPERTIES HEADER_FILE_ONLY ON )
add_library ( plugin_zstd-lib ${LIB_TYPE} ${SOURCE_LIST_CPP} ${SOURCE_LIST_C})
target_include_directories ( plugin_zstd-lib PUBLIC ${INCLUDE_LIST} )
set_property ( TARGET plugin_zstd-lib APPEND PROPERTY COMPILE_OPTIONS "${COMPILE_FLAGS_LIST}" )

# Module link
if ( plugin_zstd_DEPEND_LIST OR LINK_LIST )
  target_link_libraries ( plugin_zstd-lib ${plugin_zstd_DEPEND_LIST} ${LINK_LIST} )
endif()

# Precompiled headers settings
get_directory_property ( PCH_COMPILE_DEFINITIONS COMPILE_DEFINITIONS )
set_target_properties ( plugin_zstd-lib PROPERTIES COMPILE_FLAGS_LIST "${COMPILE_FLAGS_LIST}" )
set_target_properties ( plugin_zstd-lib PROPERTIES PCH_FILE "${PCH_FILE}" )
set_target_properties ( plugin_zstd-lib PROPERTIES PCH_INCLUDE_LIST "${INCLUDE_LIST}" )
set_target_properties ( plugin_zstd-lib PROPERTIES PCH_COMPILE_DEFINITIONS "${PCH_COMPILE_DEFINITIONS}" )

list ( LENGTH PCH_FILE PCH_FILE_LENGTH )
if ( PCH_FILE_LENGTH GREATER 1 )
  message ( FATAL_ERROR "Precompiled headers list can contain only one header file!" )
endif()
if ( PCH_FILE AND DEFINED flagPCH )
  get_filename_component ( PCH_NAME ${PCH_FILE} NAME )
  set ( PCH_DIR ${PROJECT_PCH_DIR}/plugin_zstd-lib )
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
      set_target_properties ( plugin_zstd-lib PROPERTIES COMPILE_FLAGS "-Yu${PCH_NAME} -Fp${PCH_HEADER}.pch" )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "Yc${PCH_NAME} -Fp${PCH_HEADER}.pch" )
  endif()
  if ( PCH_INCLUDE_PARAMS )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE_PARAMS}" )
  endif()
endif()

