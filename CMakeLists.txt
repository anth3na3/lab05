cmake_minimum_required(VERSION 3.4)

project(print)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

add_library(${PROJECT_NAME} STATIC sources/print.cpp)

option(BUILD_EXAMPLES "Build examples" OFF)
option(BUILD_TESTS "Build tests" OFF)

if(BUILD_TESTS)
  enable_testing()
  
  # Подавляем предупреждения компилятора внутри стороннего кода gtest
  set(COMPILE_OPTIONS_BACKUP ${CMAKE_CXX_FLAGS})
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -w")
  
  add_subdirectory(third-party/gtest)
  
  # Восстанавливаем флаги обратно для нашего кода
  set(CMAKE_CXX_FLAGS ${COMPILE_OPTIONS_BACKUP})
  
  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check ${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
