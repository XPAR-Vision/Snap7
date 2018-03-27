# Build C++ examples
macro(add_snap7_cpp_example target)
  add_executable(example_cpp_${target} EXCLUDE_FROM_ALL
    ${ARGN}
    )
  set_target_properties(example_cpp_${target} PROPERTIES
    FOLDER examples
    OUTPUT_NAME ${target}
    )
  target_link_libraries(example_cpp_${target} Snap7 Snap7Cpp)
  add_dependencies(examples example_cpp_${target})
endmacro()

add_snap7_cpp_example(client
  client.cpp
  )
add_snap7_cpp_example(server
  server.cpp
  )
add_snap7_cpp_example(srv_resourceless
  srv_resourceless.cpp
  )
add_snap7_cpp_example(apartner
  apartner.cpp
  )
add_snap7_cpp_example(ppartner
  ppartner.cpp
  )
