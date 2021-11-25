cmake_minimum_required(VERSION 3.17)
project(essentia C CXX)
set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD 20)

MACRO(HEADER_DIRECTORIES return_list)
    FILE(GLOB_RECURSE new_list  src/algorithms/*.h src/essentia/*.h)
    SET(dir_list "")
    FOREACH(file_path ${new_list})
        GET_FILENAME_COMPONENT(dir_path ${file_path} PATH)
        SET(dir_list ${dir_list} ${dir_path})
    ENDFOREACH()
    LIST(REMOVE_DUPLICATES dir_list)
    SET(${return_list} ${dir_list})
ENDMACRO()


include("cmake/sources.cmake")


HEADER_DIRECTORIES(header_dir)

file(GLOB_RECURSE SRCS ${COMP_SOURCES}    src/algorithms/essentia_algorithms_reg.cpp src/essentia)
file(GLOB_RECURSE Headers src/algorithms/*.h src/essentia/*.h)
include_directories(
        src
        src/3rdparty/eigen
        src/3rdparty
        ${header_dir}
)

add_definitions(
        -DNOMINMAX
        /bigobj
)

add_library(
        essentia  ${Headers} ${SRCS}

     )