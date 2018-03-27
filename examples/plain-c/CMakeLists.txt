# Build plain C examples
add_snap7_example(example_c_client client
  client.c
  )

add_snap7_example(example_c_server server
  server.c
  )

add_snap7_example(example_c_srv_resourceless srv_resourceless
  srv_resourceless.c
  )

add_snap7_example(example_c_apartner apartner
  apartner.c
  )

add_snap7_example(example_c_ppartner ppartner
  ppartner.c
  )
