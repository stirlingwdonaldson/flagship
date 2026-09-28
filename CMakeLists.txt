cmake_minimum_required(VERSION 3.25)

project(
  flagship
  VERSION 0.1.0
  DESCRIPTION "personal flagship project"
  LANGUAGES CXX)

# ============================================================
# C++ configuration
# ============================================================

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# ============================================================
# Dependencies
# ============================================================

set(CURSES_NEED_NCURSES TRUE)
find_package(Curses REQUIRED)

# ============================================================
# Application
# ============================================================

add_executable(flagship)

file(GLOB_RECURSE FLAGSHIP_SOURCES CONFIGURE_DEPENDS
     "${PROJECT_SOURCE_DIR}/src/*.cpp")

target_sources(flagship PRIVATE ${FLAGSHIP_SOURCES})

# ============================================================
# Include directories
# ============================================================

target_include_directories(flagship PRIVATE ${PROJECT_SOURCE_DIR}/include
                                            ${CURSES_INCLUDE_DIRS})

# ============================================================
# Libraries
# ============================================================

target_link_libraries(flagship PRIVATE ${CURSES_LIBRARIES})

# ============================================================
# Compiler warnings
# ============================================================

target_compile_options(flagship PRIVATE -Wall -Wextra -Wpedantic)

# ============================================================
# Development tools
# ============================================================

option(ENABLE_SANITIZERS "Enable AddressSanitizer and UBSan" OFF)

if(ENABLE_SANITIZERS)
  target_compile_options(flagship PRIVATE -fsanitize=address,undefined
                                          -fno-omit-frame-pointer)

  target_link_options(flagship PRIVATE -fsanitize=address,undefined)
endif()
