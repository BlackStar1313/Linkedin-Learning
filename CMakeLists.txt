cmake_minimum_required(VERSION 3.5)

project(Linkedin-Learning LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Compiler-specific flags
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    set(CMAKE_CXX_FLAGS "-g3 -Wall -O0")
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    # Add MSVC-specific flags here if needed
    add_definitions(-D_CRT_SECURE_NO_WARNINGS) # To suppress certain MSVC warnings
endif()

# List of source files
set(SOURCE_FILES
    "${PROJECT_SOURCE_DIR}/Master-C++/Standard-Template-Library(STL)/templates.cpp"
    "${PROJECT_SOURCE_DIR}/Master-C++/Standard-Template-Library(STL)/iterators.cpp"
)

foreach(file_path ${SOURCE_FILES})
    # Search for the "int main" pattern using CMake's file(STRINGS ...)
    file(STRINGS ${file_path} lines_with_main REGEX "int[ \t]*main[ \t]*\(.*\)")
    message("Grep result for ${file_path} and has main = ${lines_with_main}")
    if(lines_with_main)
        message("Pattern found in ${file_path}")
        list(APPEND SOURCE_FILES_WITH_MAIN ${file_path})
    endif()
endforeach()

foreach(file_path ${SOURCE_FILES_WITH_MAIN})
    get_filename_component(file_name ${file_path} NAME_WE)
    add_executable(${file_name} ${file_path})
endforeach()
