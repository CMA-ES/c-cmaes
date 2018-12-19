cmake_minimum_required(VERSION 2.8)
project(c-cmaes)

# Options
option(COMPILE_EXAMPLES "Compile the examples" ON)

# Build paths
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Source directory
set(CCMAES_SRC_DIR ${PROJECT_SOURCE_DIR}/src) 

# Compilation flags
set(CMAKE_C_FLAGS "-ansi -pedantic  -Wall -Wextra -Wstrict-prototypes -Wmissing-prototypes -Wshadow")

# Header directory
include_directories(CCMAES_SRC_DIR)

# Source files
set(CCMAES_SRC "${CCMAES_SRC_DIR}/cmaes.c" "${CCMAES_SRC_DIR}/boundary_transformation.c")

# Library to be compiled
add_library(ccmaes SHARED ${CCMAES_SRC})

# Link the library with the standard C lib for maths
target_link_libraries(ccmaes m)

# Examples
if(COMPILE_EXAMPLES)
    # Examples executables will be put here
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/examples)
    message(STATUS "Configuring examples")

    # Grab the examples
    add_executable(evoshort ${CCMAES_SRC_DIR}/example_short.c)
    add_executable(evobounds ${CCMAES_SRC_DIR}/example_boundary.c)

    # Link the executables with the existing ccmaes library and the standard C lib for maths
    target_link_libraries(evoshort ccmaes m)
    target_link_libraries(evobounds ccmaes m)
endif(COMPILE_EXAMPLES)
