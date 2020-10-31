cmake_minimum_required(VERSION 3.15)
project(cpp-quick-start CXX)

if (APPLE)
    message(FATAL_ERROR "OSX is currently not supported")
endif ()

if (NOT CMAKE_CXX_COMPILER_ID STREQUAL "GNU" AND
        NOT CMAKE_CXX_COMPILER_ID STREQUAL "MSVC" AND
        NOT CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    message(FATAL_ERROR "Only MSVC, GCC and Clang compilers are currently supported")
endif ()

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

add_library(project_warnings INTERFACE)

if (MSVC)
    target_compile_options(project_warnings INTERFACE /W4 /WX "/permissive-")
else ()
    message(STATUS "Building for Linux")
    target_compile_options(project_warnings INTERFACE
            -Wall
            -Werror
            -Wextra
            -Wnon-virtual-dtor
            -Wold-style-cast
            -Wcast-align
            -Wunused
            -Woverloaded-virtual
            -Wpedantic
            -Wconversion
            -Wsign-conversion
            -Wnull-dereference
            -Wdouble-promotion
            -Wformat=2)

    if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
        target_compile_options(project_warnings INTERFACE
                -Wmisleading-indentation
                -Wduplicated-cond
                -Wduplicated-branches
                -Wlogical-op
                -Wuseless-cast)
    endif ()
endif ()

add_subdirectory(src)

if (NOT DISABLE_TESTS)
    enable_testing()
    add_subdirectory(test)
endif ()
