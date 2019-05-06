# Copyright (C) 2018 Intel Corporation
# SPDX-License-Identifier: Apache-2.0
#

set (TARGET_NAME "inference_engine_c_wrapper")

file(GLOB SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
)

file(GLOB HEADERS
    ${CMAKE_CURRENT_SOURCE_DIR}/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp
)

addVersionDefines(ie_api_wrapper.cpp CI_BUILD_NUMBER)

include_directories(
    ${IE_MAIN_SOURCE_DIR}/src/inference_engine
    ${CMAKE_CURRENT_SOURCE_DIR}
)

add_definitions(-DIMPLEMENT_INFERENCE_ENGINE_API)

if (UNIX OR APPLE AND ${CMAKE_BUILD_TYPE} STREQUAL "Release")
    add_definitions(-fvisibility=default)
endif()

add_library(${TARGET_NAME} SHARED ${SOURCES} ${HEADERS})
target_link_libraries(${TARGET_NAME} inference_engine IE::ie_cpu_extension)
set_target_properties(${TARGET_NAME} PROPERTIES COMPILE_PDB_NAME ${TARGET_NAME})
