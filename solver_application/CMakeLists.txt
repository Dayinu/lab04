cmake_minimum_required(VERSION 3.5)
project(solver_app)

add_executable(solver equation.cpp)
target_link_libraries(solver PRIVATE formatter_ex solver_lib)
