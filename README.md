## Домашние задание 3

**Студентки группы ИУ8-22**

**Ивановой Влады**

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.
```sh
$ mkdir lab03_new    
                                                                                     
$ cd lab03_new    
                                                                                     
$ git clone --depth 1 https://github.com/tp-labs/lab03.git        
Cloning into 'lab03'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 19 (delta 0), reused 10 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (19/19), 1.01 MiB | 1.90 MiB/s, done.
                                                         
$ cd lab03/formatter_lib
                                                                                   
$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.5)
project(formatter)


set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter STATIC formatter.cpp)
target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

EOF                                                                         
       
$ cmake -B build     
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/linux/lab03_new/lab03/formatter_lib/build
                                                                                     
$ cmake --build build
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter

```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

```sh
$ cd ~/lab03_new/lab03/formatter_ex_lib
                                                                                     
$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.5)
project(formatter_ex)


set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_ex STATIC formatter_ex.cpp)

target_include_directories(formatter_ex PUBLIC 
    ${CMAKE_CURRENT_SOURCE_DIR} 
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
)

target_link_libraries(formatter_ex PUBLIC formatter)

EOF
        
$ cmake -B build        
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (1.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/linux/lab03_new/lab03/formatter_ex_lib/build
                                                                                     
$ cmake --build build
[ 50%] Building CXX object CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
                                  
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.
```sh

$ cd ..

$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.4)
project(formatter_project)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)

add_subdirectory(solver_application)
add_subdirectory(hello_world_application)

EOF

$ cd solver_lib

$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.4)
project(solver_lib)

add_library(solver_lib STATIC solver.cpp)
target_include_directories(solver_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

EOF

$ cd ../solver_application

$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.5)
project(solver_app)

add_executable(solver equation.cpp)
target_link_libraries(solver PRIVATE formatter_ex solver_lib)
target_include_directories(solver PRIVATE 
    ../formatter_ex_lib
    ../formatter_lib
    ../solver_lib
)

EOF

$ cd ../hello_world_application

$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.5)
project(solver_app)

add_executable(solver equation.cpp)
target_link_libraries(solver PRIVATE formatter_ex solver_lib)
target_include_directories(solver PRIVATE 
    ../formatter_ex_lib
    ../formatter_lib
    ../solver_lib
)

EOF

$ cd ..
                                                                                      
$ cmake -B build
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at formatter_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/linux/lab03_new/lab03/build
                                                                                      
$ cmake --build build 
[ 10%] Building CXX object formatter_lib/CMakeFiles/formatter.dir/formatter.cpp.o
[ 20%] Linking CXX static library libformatter.a
[ 20%] Built target formatter
[ 30%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 40%] Linking CXX static library libformatter_ex.a
[ 40%] Built target formatter_ex
[ 50%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 60%] Linking CXX static library libsolver_lib.a
[ 60%] Built target solver_lib
[ 70%] Building CXX object solver_application/CMakeFiles/solver.dir/equation.cpp.o
[ 80%] Linking CXX executable solver
[ 80%] Built target solver
[ 90%] Building CXX object hello_world_application/CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world

```

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2021 The ISC Authors
```
