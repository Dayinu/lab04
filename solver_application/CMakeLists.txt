cmake_minimum_required(VERSION 3.10)
project(solver_application)

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

add_library(solver_lib STATIC IMPORTED)
set_target_properties(solver_lib PROPERTIES
    IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/build/libsolver_lib.a
    INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib
)

add_executable(solver_application equation.cpp)

target_link_libraries(solver_application 
    PRIVATE 
    solver_lib
    formatter_ex
    formatter
    m
)

set_target_properties(solver_application PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
)
