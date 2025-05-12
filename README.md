## Домашнее задание 3

**Студентки группы ИУ8-22**

**Ивановой Влады**

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.

```sh
$ git clone --depth 1 https://github.com/tp-labs/lab03.git
Cloning into 'lab03'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 19 (delta 0), reused 10 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (19/19), 1.01 MiB | 756.00 KiB/s, done.
$ cd lab03/formatter_lib
$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.4)
project(formatter)

add_library(formatter STATIC formatter.cpp formatter.h)

target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(formatter PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED YES
)
EOF
$ mkdir build
$ cd build
$ cmake ..
Command 'cmake' not found, but can be installed with:
sudo apt install cmake
Do you want to install it? (N/y)y
sudo apt install cmake
[sudo] password for linux: 
The following packages were automatically installed and are no longer required:
  libpython3.12-dev  python3.12  python3.12-dev  python3.12-minimal  python3.12-venv
Use 'sudo apt autoremove' to remove them.

Installing:
  cmake
                                                                                       
Installing dependencies:
  cmake-data  libjsoncpp26  librhash1
                                                                                       
Suggested packages:
  cmake-doc  cmake-format  elpa-cmake-mode  ninja-build

Summary:
  Upgrading: 0, Installing: 4, Removing: 0, Not Upgrading: 1406
  Download size: 13.8 MB
  Space needed: 52.4 MB / 12.2 GB available

Continue? [Y/n] y
Get:1 http://kali.download/kali kali-rolling/main amd64 cmake-data all 3.30.5-1 [2,223 kB]
Get:2 http://kali.download/kali kali-rolling/main amd64 libjsoncpp26 amd64 1.9.6-3 [81.7 kB]
Get:3 http://kali.download/kali kali-rolling/main amd64 librhash1 amd64 1.4.5-1 [132 kB]
Get:4 http://kali.download/kali kali-rolling/main amd64 cmake amd64 3.30.5-1 [11.4 MB]
Fetched 13.8 MB in 20s (684 kB/s)                                                     
Selecting previously unselected package cmake-data.
(Reading database ... 405282 files and directories currently installed.)
Preparing to unpack .../cmake-data_3.30.5-1_all.deb ...
Unpacking cmake-data (3.30.5-1) ...
Selecting previously unselected package libjsoncpp26:amd64.
Preparing to unpack .../libjsoncpp26_1.9.6-3_amd64.deb ...
Unpacking libjsoncpp26:amd64 (1.9.6-3) ...
Selecting previously unselected package librhash1:amd64.
Preparing to unpack .../librhash1_1.4.5-1_amd64.deb ...
Unpacking librhash1:amd64 (1.4.5-1) ...
Selecting previously unselected package cmake.
Preparing to unpack .../cmake_3.30.5-1_amd64.deb ...
Unpacking cmake (3.30.5-1) ...
Setting up libjsoncpp26:amd64 (1.9.6-3) ...
Setting up cmake-data (3.30.5-1) ...
Setting up librhash1:amd64 (1.4.5-1) ...
Setting up cmake (3.30.5-1) ...
Processing triggers for libc-bin (2.40-3) ...
Processing triggers for man-db (2.13.0-1) ...
Processing triggers for kali-menu (2024.4.0) ...
$ cmake ..
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
-- Configuring done (1.8s)
-- Generating done (0.0s)
-- Build files have been written to: /home/linux/lab03/formatter_lib/build
$ make
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeList.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeList.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter.

```sh
$ cd lab03/formatter_ex_lib
$ cat > CMakeLists.txt << 'EOF'
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
EOF
$ mkdir build
$ cd build
$ cmake ..      
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
-- Configuring done (2.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/linux/lab03/formatter_ex_lib/build
$ make
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
$ cd ~/lab03/hello_world_application 
$ cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.10)
project(hello_world)

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

add_executable(hello_world hello_world.cpp)
target_link_libraries(hello_world PRIVATE formatter_ex formatter)
EOF
_link_libraries(hello_world PRIVATE formatter_ex)
EOF
$ mkdir build
$ cd build                          
$ cmake ..
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
-- Build files have been written to: /home/linux/lab03/hello_world_application/build
$ make            
[ 50%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hell
[100%] Built target hello_world
$ cd ~/lab03/solver_lib
$ cat > CMakeLists.txt << 'EOF'
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
EOF
$ mkdir build
$ cd build                     
$ cmake ..
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
CMake Deprecation Warning at /home/linux/lab03/formatter_ex_lib/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/linux/lab03/solver_lib/build    
$ make       
[ 25%] Building CXX object formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o    
[ 50%] Linking CXX static library libformatter_ex.a
[ 50%] Built target formatter_ex
[ 75%] Building CXX object CMakeFiles/solver_lib.dir/solver.cpp.o
[100%] Linking CXX static library libsolver_lib.a
[100%] Built target solver_lib
$ cd ~/lab03/solver_application
$ cat > CMakeLists.txt << 'EOF'
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
EOF
$ mkdir build                  
$ cd build                     
$ cmake ..
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
-- Build files have been written to: /home/linux/lab03/solver_application/build
$ make       
[ 50%] Building CXX object CMakeFiles/solver_application.dir/equation.cpp.o
[100%] Linking CXX executable solver_application
[100%] Built target solver_application
```
