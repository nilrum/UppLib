# CMakeLists.txt generated Thu Feb 13 15:45:13 +05 2020
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

# Module definitions
add_definitions ( -DflagMAIN )

# link(WIN32 MSC) /MAP;
if ((WIN32 AND MSVC))
  set ( MAIN_TARGET_LINK_FLAGS ${MAIN_TARGET_LINK_FLAGS} /MAP PARENT_SCOPE )
endif()

# Precompiled headers file
set ( PCH_FILE ${CMAKE_CURRENT_SOURCE_DIR}/ide.h )

list ( APPEND HEADER_LIST
      ide.h
      version.h
      UppDlg.h
      ide.key
      Assist.h
      CommandLineHandler.h
      About.h
      MethodsCtrls.h
      Methods.h
      ide.lay
      ide.iml
      ide.usc
      ide.ico
      theide.desktop
)

list ( APPEND SOURCE_LIST_CPP
      BaseDlg.cpp
      SelectPkg.cpp
      UppWspc.cpp
      NewPackageFile.cpp
      UppDlg.cpp
      Template.cpp
      Console.cpp
      FindFile.cpp
      FindInFiles.cpp
      Config.cpp
      ide.cpp
      idefile.cpp
      EditorTabBar.cpp
      Bottom.cpp
      t.cpp
      Cpp.cpp
      Assist.cpp
      DCopy.cpp
      ContextGoto.cpp
      GoToLine.cpp
      Swaps.cpp
      ParamInfo.cpp
      Navigator.cpp
      Annotations.cpp
      Virtuals.cpp
      Thisbacks.cpp
      Log.cpp
      Jump.cpp
      MainConfig.cpp
      Setup.cpp
      Custom.cpp
      Print.cpp
      Insert.cpp
      idetool.cpp
      Install.cpp
      Android.cpp
      idebar.cpp
      idewin.cpp
      main.cpp
      CommandLineHandler.cpp
      About.cpp
      Macro.cpp
      Help.cpp
      SlideShow.cpp
      OnlineSearch.cpp
      Errors.cpp
      Calc.cpp
      FormatCode.cpp
      Abbr.cpp
      Qtf.cpp
      Xml.cpp
      Json.cpp
      MacroManager.cpp
      SetupSVN.cpp
      Upgrade.cpp
      MethodsCtrls.cpp
      Methods.cpp
      AutoSetup.cpp
      InstantSetup.cpp
      OutputMode.cpp
      Build.cpp
      Debug.cpp
      Valgrind.cpp
      Export.cpp
)

list ( APPEND ide_DEPEND_LIST
      ide_Common-lib
      ide_Core-lib
      ide_LayDes-lib
      ide_IconDes-lib
      ide_Builders-lib
      ide_Debuggers-lib
      ide_Browser-lib
      CodeEditor-lib
      CtrlLib-lib
      plugin_bz2-lib
      HexView-lib
      art_BlueBar-lib
      plugin_astyle-lib
      TextDiffCtrl-lib
      TabBar-lib
      ide_Designers-lib
      ide_Android-lib
      ide_Java-lib
      ide_MacroManager-lib
      urepo-lib
      Report-lib
)

# Copy Windows resource config file to the main program build directory
file ( COPY "ide.rc" DESTINATION ${PROJECT_BINARY_DIR}/${CMAKE_PROJECT_NAME} )
file ( COPY "ide.ico" DESTINATION ${PROJECT_BINARY_DIR}/${CMAKE_PROJECT_NAME} )

# Module properties
create_cpps_from_icpps()
set_source_files_properties ( ${HEADER_LIST} PROPERTIES HEADER_FILE_ONLY ON )
add_library ( ide-lib ${LIB_TYPE} ${SOURCE_LIST_CPP} ${SOURCE_LIST_C})
target_include_directories ( ide-lib PUBLIC ${INCLUDE_LIST} )
set_property ( TARGET ide-lib APPEND PROPERTY COMPILE_OPTIONS "${COMPILE_FLAGS_LIST}" )

# Module link
if ( ide_DEPEND_LIST OR LINK_LIST )
  target_link_libraries ( ide-lib ${ide_DEPEND_LIST} ${LINK_LIST} )
endif()

# Precompiled headers settings
get_directory_property ( PCH_COMPILE_DEFINITIONS COMPILE_DEFINITIONS )
set_target_properties ( ide-lib PROPERTIES COMPILE_FLAGS_LIST "${COMPILE_FLAGS_LIST}" )
set_target_properties ( ide-lib PROPERTIES PCH_FILE "${PCH_FILE}" )
set_target_properties ( ide-lib PROPERTIES PCH_INCLUDE_LIST "${INCLUDE_LIST}" )
set_target_properties ( ide-lib PROPERTIES PCH_COMPILE_DEFINITIONS "${PCH_COMPILE_DEFINITIONS}" )

list ( LENGTH PCH_FILE PCH_FILE_LENGTH )
if ( PCH_FILE_LENGTH GREATER 1 )
  message ( FATAL_ERROR "Precompiled headers list can contain only one header file!" )
endif()
if ( PCH_FILE AND DEFINED flagPCH )
  get_filename_component ( PCH_NAME ${PCH_FILE} NAME )
  set ( PCH_DIR ${PROJECT_PCH_DIR}/ide-lib )
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
      set_target_properties ( ide-lib PROPERTIES COMPILE_FLAGS "-Yu${PCH_NAME} -Fp${PCH_HEADER}.pch" )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "Yc${PCH_NAME} -Fp${PCH_HEADER}.pch" )
  endif()
  if ( PCH_INCLUDE_PARAMS )
      set_source_files_properties ( ${SOURCE_LIST_CPP} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE_PARAMS}" )
  endif()
endif()

