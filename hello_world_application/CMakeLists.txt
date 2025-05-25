cmake_minimum_required(VERSION 3.5)
project(hello_world)

add_executable(hello_world hello_world.cpp)

# 1. Основное подключение библиотеки
target_link_libraries(hello_world PRIVATE formatter_ex)

# 2. Указание путей к заголовкам
target_include_directories(hello_world PRIVATE
    ${CMAKE_SOURCE_DIR}/formatter_ex_lib
    ${CMAKE_SOURCE_DIR}/formatter_lib
)

# 3. Настройка стандарта C++ (опционально)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
