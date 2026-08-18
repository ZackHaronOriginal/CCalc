cmake_minimum_required(VERSION 3.20)
project(CCalc VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

option(CCALC_BUILD_TESTS "Build the test suite" ON)

# Modules are added here as they gain their first real source file.
# See libs/README.md for the layer rules that govern their dependencies.
add_executable(CCalc apps/cli/main.cpp)
