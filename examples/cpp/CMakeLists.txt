# Build C++ examples
add_snap7_example(example_cpp_client client
  ../../include/snap7/snap7.cpp
  client.cpp
  )

add_snap7_example(example_cpp_server server
  ../../include/snap7/snap7.cpp
  server.cpp
  )

add_snap7_example(example_cpp_srv_resourceless srv_resourceless
  ../../include/snap7/snap7.cpp
  srv_resourceless.cpp
  )

add_snap7_example(cpp_apartner apartner
  ../../include/snap7/snap7.cpp
  apartner.cpp
  )

add_snap7_example(cpp_ppartner ppartner
  ../../include/snap7/snap7.cpp
  ppartner.cpp
  )
