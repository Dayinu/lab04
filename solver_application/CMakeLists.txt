cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver)

add_subdirectory(../formatter_ex_lib ${CMAKE_BINARY_DIR}/formatter_ex_lib)
add_subdirectory(../solver_lib ${CMAKE_BINARY_DIR}/solver_lib)

add_executable(solver equation.cpp)

target_link_libraries(solver formatter_ex solver_lib)
