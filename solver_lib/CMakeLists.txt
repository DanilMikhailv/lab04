cmake_minimum_required(VERSION 3.10)
project (solver)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_library(solver STATIC solver.cpp)
target_include_directories(solver PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
