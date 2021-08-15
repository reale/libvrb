
set (SOURCE_FILES
    vrb/src/lib/vrb/vrb_destroy.c
    vrb/src/lib/vrb/vrb_empty.c
    vrb/src/lib/vrb/vrb_get.c
    vrb/src/lib/vrb/vrb_get_min.c
    vrb/src/lib/vrb/vrb_give.c
    vrb/src/lib/vrb/vrb_header.h
    vrb/src/lib/vrb/vrb_init.c
    vrb/src/lib/vrb/vrb_lib.h
    vrb/src/lib/vrb/vrb_move.c
    vrb/src/lib/vrb/vrb_put_all.c
    vrb/src/lib/vrb/vrb_put.c
    vrb/src/lib/vrb/vrb_read.c
    vrb/src/lib/vrb/vrb_resize.c
    vrb/src/lib/vrb/vrb_take.c
    vrb/src/lib/vrb/vrb_uninit.c
    vrb/src/lib/vrb/vrb_write.c
    )

set (GEN_HEADER_DIR ${CMAKE_CURRENT_BINARY_DIR}/include)

include_directories(include)
include_directories(${GEN_HEADER_DIR})

add_executable(buildheader CONFIG/buildheader.c)

file(MAKE_DIRECTORY ${GEN_HEADER_DIR})
add_custom_command(OUTPUT ${GEN_HEADER_DIR}/vrb.h 
                   COMMAND buildheader ${GEN_HEADER_DIR}/vrb.h ${SOURCE_FILES}
                   DEPENDS buildheader 
                   WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_custom_target(generate_header ALL DEPENDS ${GEN_HEADER_DIR}/vrb.h)

add_library(libvrb SHARED ${SOURCE_FILES})
add_dependencies(libvrb generate_header)

add_executable(vbuf vrb/src/bin/vbuf.c)
target_link_libraries(vbuf libvrb)
