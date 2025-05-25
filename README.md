## Домашние задание 4

**Студентки группы ИУ8-22**

**Ивановой Влады**

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры на различных платформах:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;
* используйте [AppVeyor](https://www.appveyor.com/) для сборки на операционной системе **Windows**.
```sh
$ mkdir -p .github/workflows
$ cat > .github/workflows/ci.yml << 'EOF'
name: CI Build

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    strategy:
      matrix:
        compiler: [gcc, clang]
        build_type: [Debug, Release]
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Install dependencies
      run: |
        sudo apt-get update
        sudo apt-get install -y cmake build-essential clang
        
    - name: Set up compiler
      run: |
        if [ "${{ matrix.compiler }}" = "gcc" ]; then
          echo "CC=gcc" >> $GITHUB_ENV
          echo "CXX=g++" >> $GITHUB_ENV
        else
          echo "CC=clang" >> $GITHUB_ENV
          echo "CXX=clang++" >> $GITHUB_ENV
        fi
        
    - name: Configure CMake
      run: |
        cmake -B build -DCMAKE_BUILD_TYPE=${{ matrix.build_type }} \
          -DCMAKE_C_COMPILER=${{ env.CC }} \
          -DCMAKE_CXX_COMPILER=${{ env.CXX }}
          
    - name: Build
      run: |
        cmake --build build --config ${{ matrix.build_type }} --parallel 2
        
    - name: Run tests
      run: |
        cd build && ctest --output-on-failure
EOF

```

```
Copyright (c) 2015-2021 The ISC Authors
```
