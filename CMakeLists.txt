cmake_minimum_required(VERSION 3.1)
project(Skeleton)

# support C++14 features used by LLVM 10.0.0
set(CMAKE_CXX_STANDARD 14)

find_package(LLVM 11.1.0 REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")

add_definitions(${LLVM_DEFINITIONS})
include_directories(${LLVM_INCLUDE_DIRS})
link_directories(${LLVM_LIBRARY_DIRS})

# Add the correct include directory for LLVM 11
include_directories(/usr/lib/llvm-11/include)

add_subdirectory(skeleton)  # Use your pass name here.
