cmake_minimum_required(VERSION 3.1)

project(qdownloader
        VERSION 0.0.3
        LANGUAGES CXX)
if (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "No build type selected, default to Release")
    set(CMAKE_BUILD_TYPE "Release")
endif()

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

option(QDOWNLOADER_TESTS "Build qdownloader tests and enable testing" ON)
option(QDOWNLOADER_STATIC "Build static qdownloader library" ON)
option(QDOWNLOADER_SHARED "Build shared qdownloader library" OFF)

set(SOURCE
    src/httpclient.cpp
    src/downloader.cpp
    )

if (QDOWNLOADER_STATIC)
    add_library(qdownloader_static STATIC ${SOURCE})
    target_link_libraries(qdownloader_static curl pthread)
    target_include_directories(qdownloader_static
        PUBLIC ${PROJECT_SOURCE_DIR}/src)
endif()

if (QDOWNLOADER_SHARED)
    add_library(qdownloader SHARED ${SOURCE})
    target_link_libraries(qdownloader curl pthread)
endif()

if (QDOWNLOADER_TESTS)
    add_executable(test1 test/main.cpp)
    target_include_directories(test1
        PUBLIC ${PROJECT_SOURCE_DIR}/src)
    target_link_libraries(test1 qdownloader_static curl pthread)
endif()
