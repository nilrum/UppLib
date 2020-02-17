# CMakeLists.txt generated Thu Feb 13 15:48:18 +05 2020
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
      jpg.h
      lib/jconfig.h
      lib/cderror.h
      lib/cdjpeg.h
      lib/jdct.h
      lib/jerror.h
      lib/jinclude.h
      lib/jmemsys.h
      lib/jmorecfg.h
      lib/jpegint.h
      lib/jpeglib.h
      lib/jversion.h
      lib/transupp.h
)

list ( APPEND SOURCE_LIST_C
      lib/jaricom.c
      lib/jcapimin.c
      lib/jcapistd.c
      lib/jcarith.c
      lib/jccoefct.c
      lib/jccolor.c
      lib/jcdctmgr.c
      lib/jchuff.c
      lib/jcinit.c
      lib/jcmainct.c
      lib/jcmarker.c
      lib/jcmaster.c
      lib/jcomapi.c
      lib/jcparam.c
      lib/jcprepct.c
      lib/jcsample.c
      lib/jctrans.c
      lib/jdapimin.c
      lib/jdapistd.c
      lib/jdarith.c
      lib/jdatadst.c
      lib/jdatasrc.c
      lib/jdcoefct.c
      lib/jdcolor.c
      lib/jddctmgr.c
      lib/jdhuff.c
      lib/jdinput.c
      lib/jdmainct.c
      lib/jdmarker.c
      lib/jdmaster.c
      lib/jdmerge.c
      lib/jdpostct.c
      lib/jdsample.c
      lib/jdtrans.c
      lib/jerror.c
      lib/jfdctflt.c
      lib/jfdctfst.c
      lib/jfdctint.c
      lib/jidctflt.c
      lib/jidctfst.c
      lib/jidctint.c
      lib/jmemansi.c
      lib/jmemmgr.c
      lib/jquant1.c
      lib/jquant2.c
      lib/jutils.c
      lib/rdbmp.c
      lib/rdcolmap.c
      lib/rdgif.c
      lib/rdppm.c
      lib/rdrle.c
      lib/rdswitch.c
      lib/rdtarga.c
      lib/transupp.c
      lib/wrbmp.c
      lib/wrgif.c
      lib/wrppm.c
      lib/wrrle.c
      lib/wrtarga.c
)

list ( APPEND SOURCE_LIST_CPP
      jpgupp.cpp
)

list ( APPEND SOURCE_LIST_ICPP
      jpgreg.icpp
)

list ( APPEND plugin_jpg_DEPEND_LIST
      Draw-lib
)

# Module properties
create_cpps_from_icpps()
set_source_files_properties ( ${HEADER_LIST} PROPERTIES HEADER_FILE_ONLY ON )
add_library ( plugin_jpg-lib ${LIB_TYPE} ${SOURCE_LIST_CPP} ${SOURCE_LIST_C})
target_include_directories ( plugin_jpg-lib PUBLIC ${INCLUDE_LIST} )
set_property ( TARGET plugin_jpg-lib APPEND PROPERTY COMPILE_OPTIONS "${COMPILE_FLAGS_LIST}" )

# Module link
if ( plugin_jpg_DEPEND_LIST OR LINK_LIST )
  target_link_libraries ( plugin_jpg-lib ${plugin_jpg_DEPEND_LIST} ${LINK_LIST} )
endif()

# Precompiled headers settings
get_directory_property ( PCH_COMPILE_DEFINITIONS COMPILE_DEFINITIONS )
set_target_properties ( plugin_jpg-lib PROPERTIES COMPILE_FLAGS_LIST "${COMPILE_FLAGS_LIST}" )
set_target_properties ( plugin_jpg-lib PROPERTIES PCH_FILE "${PCH_FILE}" )
set_target_properties ( plugin_jpg-lib PROPERTIES PCH_INCLUDE_LIST "${INCLUDE_LIST}" )
set_target_properties ( plugin_jpg-lib PROPERTIES PCH_COMPILE_DEFINITIONS "${PCH_COMPILE_DEFINITIONS}" )

list ( LENGTH PCH_FILE PCH_FILE_LENGTH )
if ( PCH_FILE_LENGTH GREATER 1 )
  message ( FATAL_ERROR "Precompiled headers list can contain only one header file!" )
endif()
if ( PCH_FILE AND DEFINED flagPCH )
  get_filename_component ( PCH_NAME ${PCH_FILE} NAME )
  set ( PCH_DIR ${PROJECT_PCH_DIR}/plugin_jpg-lib )
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
      set_target_properties ( plugin_jpg-lib PROPERTIES COMPILE_FLAGS "-Yu${PCH_NAME} -Fp${PCH_HEADER}.pch" )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "Yc${PCH_NAME} -Fp${PCH_HEADER}.pch" )
  endif()
  if ( PCH_INCLUDE_PARAMS )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE_PARAMS}" )
  endif()
endif()

