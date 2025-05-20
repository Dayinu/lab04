cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex)


add_subdirectory(../formatter_lib formatter_build)

add_library(formatter_ex STATIC formatter_ex.cpp)
target_include_directories(formatter_ex PUBLIC 
    ${CMAKE_CURRENT_SOURCE_DIR}
    ../formatter_lib
)

target_link_libraries(formatter_ex PUBLIC formatter)
