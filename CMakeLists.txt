###############################################################################
#                                Configuration                                #
###############################################################################
cmake_minimum_required(VERSION 3.5)
project(snap7 VERSION 1.4.2)

# Organize things into folders for Visual Studio
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(SNAP7_INCLUDE_DIR "include/"                  CACHE PATH "Installation directory for header files")
set(SNAP7_LIBRARY_DIR "lib/"                      CACHE PATH "Installation directory for library files")
set(SNAP7_CMAKE_DIR   "lib/cmake/${PROJECT_NAME}" CACHE PATH "Installation directory for cmake config files")

# Set C++14
set(CMAKE_CXX_STANDARD 14)

# Find threading library
find_package(Threads REQUIRED)

###############################################################################
#                                Snap7 Library                                #
###############################################################################

# Snap7 base ##################################################################
# The base library only contains the plain c methods
add_library(Snap7
  src/core/s7_client.cpp
  src/core/s7_client.h
  src/core/s7_firmware.h
  src/core/s7_isotcp.cpp
  src/core/s7_isotcp.h
  src/core/s7_micro_client.cpp
  src/core/s7_micro_client.h
  src/core/s7_partner.cpp
  src/core/s7_partner.h
  src/core/s7_peer.cpp
  src/core/s7_peer.h
  src/core/s7_server.cpp
  src/core/s7_server.h
  src/core/s7_text.cpp
  src/core/s7_text.h
  src/core/s7_types.h

  $<$<BOOL:${WIN32}>:src/lib/snap7.def>
  src/lib/snap7_libmain.cpp
  src/lib/snap7_libmain.h

  src/sys/snap_msgsock.cpp
  src/sys/snap_msgsock.h
  src/sys/snap_platform.h
  src/sys/snap_sysutils.cpp
  src/sys/snap_sysutils.h
  src/sys/snap_tcpsrvr.cpp
  src/sys/snap_tcpsrvr.h
  src/sys/snap_threads.cpp
  src/sys/snap_threads.h
  src/sys/sol_threads.h
  src/sys/unix_threads.h
  src/sys/win_threads.h
  )
set_target_properties(Snap7
  PROPERTIES
  OUTPUT_NAME snap7 # Output should be libsnap7.so, not capital
  POSITION_INDEPENDENT_CODE ON # -fPIC
  COMPILE_FLAGS "-O3"
  )
target_compile_definitions(Snap7
  PUBLIC
  $<$<BOOL:${WIN32}>:_WIN32_WINNT=0x0501>
  $<$<BOOL:${WIN32}>:WIN32_LEAN_AND_MEAN>
  $<$<BOOL:${WIN32}>:_WINSOCK_DEPRECATED_NO_WARNINGS>
  $<$<BOOL:${WIN32}>:_CRT_SECURE_NO_WARNINGS>

  PRIVATE
  $<$<BOOL:${WIN32}>:_USRDLL>
  )
target_include_directories(Snap7
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/snap7/>
  $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include/snap7/>

  PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/src/core/
  ${CMAKE_CURRENT_SOURCE_DIR}/src/lib/
  ${CMAKE_CURRENT_SOURCE_DIR}/src/sys/
  )
target_link_libraries(Snap7
  PUBLIC
  ${CMAKE_THREAD_LIBS_INIT}
  # Windows
  $<$<BOOL:${WIN32}>:Ws2_32.lib>
  $<$<BOOL:${WIN32}>:winmm.lib>
  # Linux
  $<$<AND:$<BOOL:${UNIX}>,$<NOT:$<BOOL:${APPLE}>>>:rt> # Real-time extension
  )

# Snap7 wrapper ###############################################################
# The c++ library contains the wrapper object over the public plain c interface
add_library(Snap7Wrapper
  src/snap7.cpp
  )
set_target_properties(Snap7Wrapper
  PROPERTIES
  OUTPUT_NAME snap7wrapper
  POSITION_INDEPENDENT_CODE ON # -fPIC
  COMPILE_FLAGS "-O3"
  )
target_link_libraries(Snap7Wrapper PUBLIC Snap7)

# Install #####################################################################
# Install libraries; create an export target "Snap7Config" that remembers where
# we installed them
install(TARGETS Snap7 Snap7Wrapper EXPORT Snap7Config
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  )

# Install headers
install(DIRECTORY include/snap7/ DESTINATION include/snap7/)

# This makes the libraries importable from the install directory
install(EXPORT Snap7Config
  NAMESPACE Snap7::
  DESTINATION lib/cmake/snap7/)

# This makes the libraries importable from the build directory without installing
export(TARGETS Snap7 Snap7Wrapper FILE Snap7Config.cmake)

###############################################################################
#                                   Examples                                  #
###############################################################################
add_subdirectory(examples)

