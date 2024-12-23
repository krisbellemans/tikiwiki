# Minimum CMake version required
cmake_minimum_required(VERSION 3.10)

# Project name and language
project(Wiki LANGUAGES CXX)

# Specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Specify the compiler explicitly (use clang++)
set(CMAKE_CXX_COMPILER clang++)

# Add the source directory
add_subdirectory(src)

# Optional: Additional configuration flags for clang++
if(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    add_compile_options(
        -Wall
        -Wextra
        -Wpedantic
        -Werror
    )
endif()
