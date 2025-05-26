## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в прошлый раз. Настройте сборочные процедуры на различных платформах:

1. используйте TravisCI для сборки на операционной системе Linux с использованием компиляторов gcc и clang;

2. используйте AppVeyor для сборки на операционной системе Windows.

**Структура проекта**

```
lab04/
├── CMakeLists.txt
├── formatter_ex_lib
│   ├── CMakeLists.txt
│   ├── formatter_ex.cpp
│   └── formatter_ex.h
├── formatter_lib
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 3.30.5
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── a.out
│   │   │   │   └── CMakeCCompilerId.c
│   │   │   └── CompilerIdCXX
│   │   │       ├── a.out
│   │   │       └── CMakeCXXCompilerId.cpp
│   │   ├── cmake.check_cache
│   │   └── CMakeConfigureLog.yaml
│   ├── CMakeLists.txt
│   ├── formatter.cpp
│   └── formatter.h
├── hello_world_application
│   ├── CMakeLists.txt
│   └── hello_world.cpp
├── LICENSE
├── preview.png
├── README.md
├── solver_application
│   └── equation.cpp
└── solver_lib
    ├── CMakeLists.txt
    ├── solver.cpp
    └── solver.h
```

Корневой CMake для сборки:

CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.10)
project(builder)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)
cmake_policy(SET CMP0079 NEW)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)

add_executable(hello_world ${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)
add_executable(equation ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)

target_link_libraries(formatter_ex formatter)
target_link_libraries(hello_world formatter_ex)
target_link_libraries(equation formatter_ex)
target_link_libraries(equation solver)
```

cmake.yml:

```
name: GitHub Actions 
on: [push]
jobs:
  run_on_Linux:
      runs-on: ubuntu-latest
      steps:
        - name: Checkout
          uses: actions/checkout@v4
          
        - run: |
               cmake -H. -B_build
               cmake --build _build
               
        - name: Configure and Build Project
          uses: threeal/cmake-action@v1.3.0
          
  run_on_Windows:
        runs-on: windows-latest
        steps:
          - name: Checkout
            uses: actions/checkout@v4
            
          - name: Set up MSBuild
            uses: microsoft/setup-msbuild@v1.0.2
            
          - run: |
              mkdir _build
              cd _build
              cmake ..
              cmake --build .
              
          - name: Configure and Build Project
            uses: threeal/cmake-action@v1.3.0
```

Локальная сборка:

```
mkdir build && cd build
cmake ..
make
```

Далее сбока происходит на GitHub с помощью GitHub actions

![image](https://github.com/user-attachments/assets/809361a1-bbe1-4e00-9664-c8fe8ba5b9cc)





