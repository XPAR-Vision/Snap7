###############################################################################
#                                   Examples                                  #
###############################################################################
# Build examples using "make examples"
add_custom_target(examples)

# Quick macro to define example executables
macro(add_snap7_example target output_name)
  add_executable(${target} EXCLUDE_FROM_ALL
    ${ARGN}
    )
  set_target_properties(${target} PROPERTIES
    FOLDER examples
    OUTPUT_NAME ${output_name}
    )
  target_link_libraries(${target} snap7)
  add_dependencies(examples ${target})
endmacro()

# Separate into c++ and plain c examples
add_subdirectory(cpp)
add_subdirectory(plain-c)

