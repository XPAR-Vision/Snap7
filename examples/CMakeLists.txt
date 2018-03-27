###############################################################################
#                                   Examples                                  #
###############################################################################
# Build examples using "make examples"
add_custom_target(examples)

# Separate into c++ and plain c examples
add_subdirectory(cpp)
add_subdirectory(plain-c)

