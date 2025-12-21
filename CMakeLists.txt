cmake_minimum_required(VERSION 3.25)
project(onsetsds~)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(PDCMAKE_FILE ${CMAKE_BINARY_DIR}/pd.cmake)
set(PDCMAKE_VERSION "v0.2.11")
if(NOT EXISTS "${PDCMAKE_FILE}")
    file(DOWNLOAD 
    https://github.com/pure-data/pd.cmake/releases/download/v2.0.11/pd.cmake ${PDCMAKE_FILE})
endif()
include(${PDCMAKE_FILE})

file(GLOB ONSETSDS_SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c")
pd_add_external(onsetsds~ "${ONSETSDS_SRC}")
pd_add_datafile(onsetsds~ "${CMAKE_CURRENT_SOURCE_DIR}/onsetsds~-help.pd")
