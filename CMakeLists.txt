cmake_minimum_required(VERSION 3.13)
project(quickjspp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)

if(CMAKE_COMPILER_IS_GNUCC)
    add_compile_options(-Wall -Wno-unused-parameter)
endif()

#set(qjs_DIR quickjs/windows/lib/cmake/quickjs)
#find_package(qjs CONFIG REQUIRED)

add_library(quickjspp INTERFACE)
target_link_directories(quickjspp INTERFACE . quickjs/windows/lib)
target_link_libraries(quickjspp INTERFACE qjs ${CMAKE_REQUIRED_LIBRARIES})
target_compile_features(quickjspp INTERFACE cxx_std_17)
target_include_directories(quickjspp INTERFACE . quickjs/windows/include)
set_target_properties(quickjspp PROPERTIES PUBLIC_HEADER quickjspp.hpp)

add_executable(qjsp qjs.cpp)
target_link_libraries(qjsp quickjspp qjs)

# cmake -DBUILD_TESTING=OFF to disable tests
#include(CTest)
#if(BUILD_TESTING)
#    add_subdirectory(test)
#endif()

install(TARGETS quickjspp qjsp)
