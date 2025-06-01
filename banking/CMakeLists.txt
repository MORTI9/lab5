cmake_minimum_required(VERSION 3.14)
project(banking LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(financial-engine
    Account.cpp
    Account.h
    Transaction.cpp
    Transaction.h
)

target_include_directories(financial-engine PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

option(BUILD_UNIT_TESTS "Build unit tests for the project" ON)

if(BUILD_UNIT_TESTS)
    enable_testing()

    add_subdirectory(third-party/gtest)

    file(GLOB TEST_CASES tests/*.cpp)

    add_executable(test-runner ${TEST_CASES})

    target_link_libraries(test-runner
        financial-engine
        gtest_main
        gmock_main
    )

    add_test(NAME test-runner COMMAND test-runner)
endif()

option(ENABLE_TEST_COVERAGE "Enable code coverage instrumentation" OFF)

if(ENABLE_TEST_COVERAGE)
    message(STATUS "Code coverage support is active")
    add_compile_options(-fprofile-arcs -ftest-coverage)
    add_link_options(--coverage)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
endif()
