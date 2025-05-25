# Корректный корневой CMakeLists.txt

cmake_minimum_required(VERSION 3.10)
project(Lab03)

# Подключаем библиотеки
add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)

# Подключаем приложения
add_subdirectory(hello_world_application)
add_subdirectory(solver_application)
