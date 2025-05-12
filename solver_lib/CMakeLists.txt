cmake_minimum_required(VERSION 3.10)
project(solver_lib)

add_subdirectory(../formatter_ex_lib formatter_ex)

add_library(solver_lib STATIC solver.cpp solver.h)

target_link_libraries(solver_lib 
    PRIVATE 
    formatter_ex
    m  # Математическая библиотека
)

set_target_properties(solver_lib PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
)
