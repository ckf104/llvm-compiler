cmake_minimum_required(VERSION 3.0.0)
project(compiler VERSION 0.1.0)
set(CMAKE_CXX_STANDARD 17)
# find_package() include(CTest) enable_testing()

set(llvmdir "/home/ckf/llvm-project-llvmorg-14.0.3/build_llvm/lib/cmake/llvm/")
set(CMAKE_BUILD_TYPE "Debug")
find_package(LLVM REQUIRED PATH_SUFFIXES ${llvmdir})

if(LLVM_FOUND)
  separate_arguments(DEF UNIX_COMMAND ${LLVM_DEFINITIONS})
  add_executable(qq_compiler main.cpp)
  target_compile_definitions(qq_compiler PRIVATE DEF)
  target_include_directories(qq_compiler PRIVATE ${LLVM_INCLUDE_DIRS})
  # message(STATUS ${LLVM_INCLUDE_DIRS})
  llvm_map_components_to_libnames(
    llvm_libs
    support
    core
    analysis
    passes
    ExecutionEngine
    orcjit
    executionengine
    native
    scalaropts
    instcombine
    transformutils
    mc)
  target_link_libraries(qq_compiler PRIVATE ${llvm_libs})
else()
  message(FATAL_ERROR "can't find llvm")
endif()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
# include(CPack)
