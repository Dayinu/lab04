cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(hello_world)

add_subdirectory(../formatter_ex_lib ${CMAKE_BINARY_DIR}/formatter_ex_lib)

add_executable(hello_world hello_world.cpp)

target_link_libraries(hello_world formatter_ex)
