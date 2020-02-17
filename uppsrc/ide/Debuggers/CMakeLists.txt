# CMakeLists.txt generated Thu Feb 13 15:45:48 +05 2020
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

# uses(!POSIX) plugin/ndisasm;
if ((NOT DEFINED flagPOSIX))
endif()

# library(WIN32 GCC) "dbghelp psapi";
if ((WIN32 AND CMAKE_COMPILER_IS_GNUCC))
  list ( APPEND LINK_LIST dbghelp psapi )
endif()

# Precompiled headers file
set ( PCH_FILE ${CMAKE_CURRENT_SOURCE_DIR}/Debuggers.h )

list ( APPEND HEADER_LIST
      Debuggers.h
      Debuggers.iml
      Gdb.lay
      Gdb.h
      GdbUtils.h
      Pdb.h
      cvconst.h
      i386.cpu
      amd64.cpu
      Pdb.lay
      Pdb.key
)

list ( APPEND SOURCE_LIST_CPP
      Terminal.cpp
      Disas.cpp
      GdbCmd.cpp
      GdbData.cpp
      Gdb.cpp
      GdbMem.cpp
      GdbUtils.cpp
      Cpu.cpp
      Debug.cpp
      Mem.cpp
      Sym.cpp
      Exp.cpp
      Visualise.cpp
      Data.cpp
      Tree.cpp
      Stack.cpp
      Code.cpp
      Pdb.cpp
)

list ( APPEND ide_Debuggers_DEPEND_LIST
      ide_Common-lib
      HexView-lib
      ide_Core-lib
)

# Module properties
create_cpps_from_icpps()
set_source_files_properties ( ${HEADER_LIST} PROPERTIES HEADER_FILE_ONLY ON )
add_library ( ide_Debuggers-lib ${LIB_TYPE} ${SOURCE_LIST_CPP} ${SOURCE_LIST_C})
target_include_directories ( ide_Debuggers-lib PUBLIC ${INCLUDE_LIST} )
set_property ( TARGET ide_Debuggers-lib APPEND PROPERTY COMPILE_OPTIONS "${COMPILE_FLAGS_LIST}" )

# Module link
if ( ide_Debuggers_DEPEND_LIST OR LINK_LIST )
  target_link_libraries ( ide_Debuggers-lib ${ide_Debuggers_DEPEND_LIST} ${LINK_LIST} )
endif()

# Precompiled headers settings
get_directory_property ( PCH_COMPILE_DEFINITIONS COMPILE_DEFINITIONS )
set_target_properties ( ide_Debuggers-lib PROPERTIES COMPILE_FLAGS_LIST "${COMPILE_FLAGS_LIST}" )
set_target_properties ( ide_Debuggers-lib PROPERTIES PCH_FILE "${PCH_FILE}" )
set_target_properties ( ide_Debuggers-lib PROPERTIES PCH_INCLUDE_LIST "${INCLUDE_LIST}" )
set_target_properties ( ide_Debuggers-lib PROPERTIES PCH_COMPILE_DEFINITIONS "${PCH_COMPILE_DEFINITIONS}" )

list ( LENGTH PCH_FILE PCH_FILE_LENGTH )
if ( PCH_FILE_LENGTH GREATER 1 )
  message ( FATAL_ERROR "Precompiled headers list can contain only one header file!" )
endif()
if ( PCH_FILE AND DEFINED flagPCH )
  get_filename_component ( PCH_NAME ${PCH_FILE} NAME )
  set ( PCH_DIR ${PROJECT_PCH_DIR}/ide_Debuggers-lib )
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
      set_target_properties ( ide_Debuggers-lib PROPERTIES COMPILE_FLAGS "-Yu${PCH_NAME} -Fp${PCH_HEADER}.pch" )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "Yc${PCH_NAME} -Fp${PCH_HEADER}.pch" )
  endif()
  if ( PCH_INCLUDE_PARAMS )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE_PARAMS}" )
  endif()
endif()

