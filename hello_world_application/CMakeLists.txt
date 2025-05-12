cmake_minimum_required(VERSION 3.10)
project(hello_world)

add_library(formatter STATIC IMPORTED)
set_target_properties(formatter PROPERTIES
    IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib/build/libformatter.a
    INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
)

add_library(formatter_ex STATIC IMPORTED)
set_target_properties(formatter_ex PROPERTIES
    IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/build/libformatter_ex.a
    INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
)

add_executable(hello_world hello_world.cpp)
target_link_libraries(hello_world PRIVATE formatter_ex formatter)
