cmake_minimum_required(VERSION 3.4)
project(formatter_ex)

add_library(formatter STATIC IMPORTED)
set_target_properties(formatter PROPERTIES
    IMPORTED_LOCATION /home/linux/lab03/formatter_lib/build/libformatter.a
    INTERFACE_INCLUDE_DIRECTORIES /home/linux/lab03/formatter_lib
)

add_library(formatter_ex STATIC formatter_ex.cpp formatter_ex.h)

target_include_directories(formatter_ex PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(formatter_ex PUBLIC formatter)

set_target_properties(formatter_ex PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
)
