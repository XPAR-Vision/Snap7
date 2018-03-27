# Build plain C examples
macro(add_snap7_c_example target)
  add_executable(example_c_${target} EXCLUDE_FROM_ALL
    ${ARGN}
    )
  set_target_properties(example_c_${target} PROPERTIES
    FOLDER examples
    OUTPUT_NAME ${target}
    )
  target_link_libraries(example_c_${target} Snap7)
  add_dependencies(examples example_c_${target})
endmacro()

add_snap7_c_example(client
  client.c
  )
add_snap7_c_example(server
  server.c
  )
add_snap7_c_example(srv_resourceless
  srv_resourceless.c
  )
add_snap7_c_example(apartner
  apartner.c
  )
add_snap7_c_example(ppartner
  ppartner.c
  )
