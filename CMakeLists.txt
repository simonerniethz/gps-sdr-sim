cmake_minimum_required(VERSION 3.16)

project(gps-sdr-sim)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

include(FetchContent)


FetchContent_Declare(
        spdlog
        GIT_REPOSITORY https://github.com/gabime/spdlog.git
        GIT_TAG v1.9.2
)
FetchContent_GetProperties(spdlog)


if (NOT spdlog_POPULATED)
    FetchContent_Populate(spdlog)

    add_subdirectory(${spdlog_SOURCE_DIR})
endif ()


add_executable(gps-sdr-sim gpssim.cpp gpssim.h)
target_link_libraries(gps-sdr-sim
    pthread
    uhd
    spdlog::spdlog
)