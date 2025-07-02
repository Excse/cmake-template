cmake_minimum_required(VERSION 3.31)
project(cmake_template)

set(CMAKE_CXX_STANDARD 20)

add_library(cmake_template STATIC library.cpp)
