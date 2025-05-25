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
