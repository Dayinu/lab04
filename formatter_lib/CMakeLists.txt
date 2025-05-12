cmake_minimum_required(VERSION 3.4)
project(formatter)

add_library(formatter STATIC formatter.cpp formatter.h)

target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(formatter PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED YES
)
