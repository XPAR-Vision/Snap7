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
# Private source and headers
file(GLOB_RECURSE cxx_sources src/*.cpp)
file(GLOB_RECURSE cxx_headers src/*.h)

# Build library
add_library(snap7 SHARED ${cxx_sources} ${cxx_headers})
set_target_properties(snap7
  PROPERTIES
  POSITION_INDEPENDENT_CODE ON # -fPIC
  COMPILE_FLAGS "-O3"
  )
target_include_directories(snap7
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/snap7/>
  $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include/snap7/>

  PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/src/core/
  ${CMAKE_CURRENT_SOURCE_DIR}/src/lib/
  ${CMAKE_CURRENT_SOURCE_DIR}/src/sys/
  )
target_link_libraries(snap7
  PUBLIC
  ${CMAKE_THREAD_LIBS_INIT}
  # Windows
  $<$<BOOL:${WIN32}>:Ws2_32.lib>
  $<$<BOOL:${WIN32}>:winmm.lib>
  # Linux
  $<$<AND:$<BOOL:${UNIX}>,$<NOT:$<BOOL:${APPLE}>>>:rt> # Real-time extension
  )

# Install library, create an export target "snap7Config"
install(TARGETS snap7 EXPORT snap7Config
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
  )

# Install public header files
# Use "DIRECTORY" signature to preserve folder structure
install(DIRECTORY ./include/ DESTINATION "${SNAP7_INCLUDE_DIR}"
    FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ./include/ DESTINATION "${SNAP7_INCLUDE_DIR}"
    FILES_MATCHING PATTERN "*.cpp")

install(EXPORT snap7Config DESTINATION "${SNAP7_CMAKE_DIR}")

# This makes the project importable from the build directory without installing
export(TARGETS snap7 FILE snap7Config.cmake)

###############################################################################
#                                   Examples                                  #
###############################################################################
add_subdirectory(examples)

