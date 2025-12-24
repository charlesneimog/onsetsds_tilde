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

cmake_policy(SET CMP0135 NEW)
set(BUILD_SHARED_LIBS
    OFF
    CACHE BOOL "Build static libs" FORCE)
set(BUILD_TESTS
    OFF
    CACHE BOOL "Disable FFTW tests" FORCE)
set(ENABLE_FLOAT
    ON
    CACHE BOOL "Enable single precision" FORCE)
set(FFTW3_FILE ${CMAKE_BINARY_DIR}/fftw-3.3.10.tar.gz)
if(NOT EXISTS ${FFTW3_FILE})
    message(STATUS "Downloading FFTW3")
    file(DOWNLOAD https://www.fftw.org/fftw-3.3.10.tar.gz ${FFTW3_FILE} STATUS DOWNLOAD_STATUS)
endif()

file(ARCHIVE_EXTRACT INPUT ${CMAKE_BINARY_DIR}/fftw-3.3.10.tar.gz DESTINATION ${CMAKE_BINARY_DIR}/)
add_subdirectory(${CMAKE_BINARY_DIR}/fftw-3.3.10 ${CMAKE_BINARY_DIR}/fftw3f EXCLUDE_FROM_ALL)
set_target_properties(fftw3f PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR})
set_target_properties(fftw3f PROPERTIES POSITION_INDEPENDENT_CODE ON)

file(GLOB ONSETSDS_SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c")
pd_add_external(onsetsds~ "${ONSETSDS_SRC}" LINK_LIBRARIES fftw3f)
pd_add_datafile(onsetsds~ "${CMAKE_CURRENT_SOURCE_DIR}/resources/onsetsds~-help.pd")
set_target_properties(
    onsetsds_tilde
    PROPERTIES C_STANDARD 23
               C_STANDARD_REQUIRED YES
               C_EXTENSIONS NO)
