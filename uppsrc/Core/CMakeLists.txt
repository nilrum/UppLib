# CMakeLists.txt generated Fri Feb 14 11:53:38 +05 2020
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

# uses(WIN32) plugin/z;
if ((WIN32))
endif()

# library(LINUX !ANDROID) "pthread dl rt";
if ((${CMAKE_SYSTEM_NAME} MATCHES Linux AND NOT ANDROID))
  find_package ( Threads REQUIRED  )
  if ( THREADS_FOUND )
      list ( APPEND INCLUDE_LIST ${THREADS_INCLUDE_DIRS} )
      list ( APPEND LINK_LIST ${THREADS_LIBRARIES} )
      # remove leading or trailing whitespace (e.g. for SDL2)
      if ( LINK_LIST )
          string ( STRIP "${LINK_LIST}" LINK_LIST )
      endif()
      if ( CMAKE_THREAD_LIBS_INIT )
          list ( APPEND LINK_LIST ${CMAKE_THREAD_LIBS_INIT} )
      endif()
  endif()
  list ( APPEND LINK_LIST dl rt )
endif()

# library(BSD !OSX) "pthread rt execinfo";
if ((${CMAKE_SYSTEM_NAME} MATCHES BSD AND NOT OSX))
  find_package ( Threads REQUIRED  )
  if ( THREADS_FOUND )
      list ( APPEND INCLUDE_LIST ${THREADS_INCLUDE_DIRS} )
      list ( APPEND LINK_LIST ${THREADS_LIBRARIES} )
      # remove leading or trailing whitespace (e.g. for SDL2)
      if ( LINK_LIST )
          string ( STRIP "${LINK_LIST}" LINK_LIST )
      endif()
      if ( CMAKE_THREAD_LIBS_INIT )
          list ( APPEND LINK_LIST ${CMAKE_THREAD_LIBS_INIT} )
      endif()
  endif()
  list ( APPEND LINK_LIST rt execinfo )
endif()

# library(WIN32 !MSC8ARM) "advapi32 shell32 winmm mpr";
if ((WIN32 AND NOT DEFINED flagMSC8ARM))
  list ( APPEND LINK_LIST advapi32 shell32 winmm mpr )
endif()

# library(SOLARIS) "posix4 dl";
if ((${CMAKE_SYSTEM_NAME} MATCHES Solaris))
  list ( APPEND LINK_LIST posix4 dl )
endif()

# library(!WIN32) z;
if ((NOT WIN32))
  list ( APPEND LINK_LIST z )
endif()

# library(OSX) pthread;
if ((OSX))
  find_package ( Threads REQUIRED  )
  if ( THREADS_FOUND )
      list ( APPEND INCLUDE_LIST ${THREADS_INCLUDE_DIRS} )
      list ( APPEND LINK_LIST ${THREADS_LIBRARIES} )
      # remove leading or trailing whitespace (e.g. for SDL2)
      if ( LINK_LIST )
          string ( STRIP "${LINK_LIST}" LINK_LIST )
      endif()
      if ( CMAKE_THREAD_LIBS_INIT )
          list ( APPEND LINK_LIST ${CMAKE_THREAD_LIBS_INIT} )
      endif()
  endif()
endif()

# library(ANDROID) atomic;
if ((ANDROID))
  list ( APPEND LINK_LIST atomic )
endif()

# library(WIN32) "kernel32 user32 ole32 oleaut32 uuid ws2_32";
if ((WIN32))
  list ( APPEND LINK_LIST kernel32 user32 ole32 oleaut32 uuid ws2_32 )
endif()

# library(ANDROID) cpufeatures;
if ((ANDROID))
  list ( APPEND LINK_LIST cpufeatures )
endif()

# options(XGNU) -O0;
if ((CMAKE_COMPILER_IS_GNUCXX))
  list ( APPEND COMPILE_FLAGS_LIST -O0 )
endif()

# options(XGNU) -ffunction-sections;
if ((CMAKE_COMPILER_IS_GNUCXX))
  list ( APPEND COMPILE_FLAGS_LIST -ffunction-sections )
endif()

# link(SOLARIS) "-Wl,-R -Wl,/usr/local/lib";
if ((${CMAKE_SYSTEM_NAME} MATCHES Solaris))
  set ( MAIN_TARGET_LINK_FLAGS ${MAIN_TARGET_LINK_FLAGS} -Wl,-R -Wl,/usr/local/lib PARENT_SCOPE )
endif()

# link(GCC POSIX STACKTRACE) -rdynamic;
if ((CMAKE_COMPILER_IS_GNUCC AND DEFINED flagPOSIX AND DEFINED flagSTACKTRACE))
  set ( MAIN_TARGET_LINK_FLAGS ${MAIN_TARGET_LINK_FLAGS} -rdynamic PARENT_SCOPE )
endif()

# Precompiled headers file
set ( PCH_FILE ${CMAKE_CURRENT_SOURCE_DIR}/Core.h )

list ( APPEND HEADER_LIST
      Core.h
      config.h
      Defs.h
      Ops.h
      Atomic.h
      Mt.h
      Heap.h
      HeapImp.h
      String.h
      AString.hpp
      SplitMerge.h
      CharSet.i
      CharSet.h
      Utf.hpp
      Path.h
      App.h
      Huge.h
      Stream.h
      FilterStream.h
      Profile.h
      Diag.h
      Util.h
      LocalProcess.h
      Topt.h
      Range.h
      Algo.h
      CoAlgo.h
      Sorted.h
      Sort.h
      CoSort.h
      Obsolete.h
      Vcont.h
      BiCont.h
      Other.h
      Vcont.hpp
      Index.h
      Index.hpp
      Map.h
      Map.hpp
      FixedMap.h
      InVector.h
      InVector.hpp
      InMap.hpp
      Tuple.h
      Function.h
      CallbackNP.i
      CallbackN.i
      CallbackR.i
      Callback.h
      TimeDate.h
      Value.h
      Value.hpp
      ValueUtil.h
      ValueUtil.hpp
      Complex.h
      Format.h
      Convert.h
      Color.h
      Gtypes.h
      i18n.h
      Core.t
      t.h
      Lang.h
      lcid.txt
      Parser.h
      XML.h
      Xmlize.h
      Xmlize.hpp
      JSON.h
      Uuid.h
      Ptr.h
      z.h
      Topic.h
      topic_group.h
      CoWork.h
      Hash.h
      Inet.h
      dli.h
      dli_header.h
      dli_source.h
      Win32Util.h
      sub.upt
      core.upt
      core.defs
)

list ( APPEND SOURCE_LIST_C
      lib/lz4.c
      lib/xxhash.c
)

list ( APPEND SOURCE_LIST_CPP
      Cpu.cpp
      Mt.cpp
      heaputil.cpp
      hheap.cpp
      sheap.cpp
      lheap.cpp
      heap.cpp
      heapdbg.cpp
      StringFind.cpp
      String.cpp
      WString.cpp
      StrUtil.cpp
      SplitMerge.cpp
      Utf.cpp
      UnicodeInfo.cpp
      CharSet.cpp
      Bom.cpp
      Path.cpp
      NetNode.cpp
      App.cpp
      Huge.cpp
      Stream.cpp
      BlockStream.cpp
      FileMapping.cpp
      FilterStream.cpp
      Log.cpp
      Debug.cpp
      Ini.cpp
      Util.cpp
      mathutil.cpp
      Random.cpp
      Speller.cpp
      LocalProcess.cpp
      Vcont.cpp
      Index.cpp
      TimeDate.cpp
      Value.cpp
      ValueUtil.cpp
      Format.cpp
      Convert.cpp
      Color.cpp
      Gtypes.cpp
      t.cpp
      Lang.cpp
      LangInfo.cpp
      parser.cpp
      XML.cpp
      Xmlize.cpp
      JSON.cpp
      Uuid.cpp
      Ptr.cpp
      z.cpp
      Topic.cpp
      CoWork.cpp
      MD5.cpp
      SHA1.cpp
      SHA256.cpp
      xxHsh.cpp
      InetUtil.cpp
      MIME.cpp
      Socket.cpp
      Http.cpp
      WebSocket.cpp
      Dli.cpp
      Win32Util.cpp
)

# Module properties
create_cpps_from_icpps()
set_source_files_properties ( ${HEADER_LIST} PROPERTIES HEADER_FILE_ONLY ON )
add_library ( Core-lib ${LIB_TYPE} ${SOURCE_LIST_CPP} ${SOURCE_LIST_C})
target_include_directories ( Core-lib PUBLIC ${INCLUDE_LIST} )
set_property ( TARGET Core-lib APPEND PROPERTY COMPILE_OPTIONS "${COMPILE_FLAGS_LIST}" )

# Module link
if ( Core_DEPEND_LIST OR LINK_LIST )
  target_link_libraries ( Core-lib ${Core_DEPEND_LIST} ${LINK_LIST} )
endif()

# Precompiled headers settings
get_directory_property ( PCH_COMPILE_DEFINITIONS COMPILE_DEFINITIONS )
set_target_properties ( Core-lib PROPERTIES COMPILE_FLAGS_LIST "${COMPILE_FLAGS_LIST}" )
set_target_properties ( Core-lib PROPERTIES PCH_FILE "${PCH_FILE}" )
set_target_properties ( Core-lib PROPERTIES PCH_INCLUDE_LIST "${INCLUDE_LIST}" )
set_target_properties ( Core-lib PROPERTIES PCH_COMPILE_DEFINITIONS "${PCH_COMPILE_DEFINITIONS}" )

list ( LENGTH PCH_FILE PCH_FILE_LENGTH )
if ( PCH_FILE_LENGTH GREATER 1 )
  message ( FATAL_ERROR "Precompiled headers list can contain only one header file!" )
endif()
if ( PCH_FILE AND DEFINED flagPCH )
  get_filename_component ( PCH_NAME ${PCH_FILE} NAME )
  set ( PCH_DIR ${PROJECT_PCH_DIR}/Core-lib )
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
      set_target_properties ( Core-lib PROPERTIES COMPILE_FLAGS "-Yu${PCH_NAME} -Fp${PCH_HEADER}.pch" )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "Yc${PCH_NAME} -Fp${PCH_HEADER}.pch" )
  endif()
  if ( PCH_INCLUDE_PARAMS )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE_PARAMS}" )
  endif()
endif()

