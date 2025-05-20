cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter)

add_library(formatter STATIC 
    ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp
)

# Устанавливаем пути к заголовочным файлам
target_include_directories(formatter PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

install(TARGETS formatter
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(FILES formatter.h DESTINATION include)
