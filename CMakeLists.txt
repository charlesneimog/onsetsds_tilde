cmake_minimum_required(VERSION 3.25)
project(onsetsds~)

set(CMAKE_POLICY_VERSION_MINIMUM 3.5)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(PDCMAKE_FILE ${CMAKE_BINARY_DIR}/pd.cmake)
set(PDCMAKE_VERSION "v0.2.11")
if(NOT EXISTS "${PDCMAKE_FILE}")
    file(DOWNLOAD https://github.com/pure-data/pd.cmake/releases/download/v2.0.11/pd.cmake ${PDCMAKE_FILE})
endif()
include(${PDCMAKE_FILE})


add_library(onsetsds STATIC "${CMAKE_CURRENT_SOURCE_DIR}/src/onsetsds.c")
target_include_directories(onsetsds PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src/")

pd_add_external(onsetsds~ "${CMAKE_CURRENT_SOURCE_DIR}/src/onsetsds~.c" LINK_LIBRARIES onsetsds)
pd_add_datafile(onsetsds~ "${CMAKE_CURRENT_SOURCE_DIR}/resources/onsetsds~-help.pd")
set_target_properties(
    onsetsds_tilde
    PROPERTIES C_STANDARD 23
               C_STANDARD_REQUIRED YES
               C_EXTENSIONS NO)
