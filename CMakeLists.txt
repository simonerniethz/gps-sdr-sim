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

set(BOOST_REQUIRED_COMPONENTS
        program_options
        )
if (UNIX AND EXISTS "/usr/lib64")
    list(APPEND BOOST_LIBRARYDIR "/usr/lib64") #fedora 64-bit fix
endif (UNIX AND EXISTS "/usr/lib64")
set(Boost_ADDITIONAL_VERSIONS
        "1.35.0" "1.35" "1.36.0" "1.36" "1.37.0" "1.37" "1.38.0" "1.38" "1.39.0" "1.39"
        "1.40.0" "1.40" "1.41.0" "1.41" "1.42.0" "1.42" "1.43.0" "1.43" "1.44.0" "1.44"
        "1.45.0" "1.45" "1.46.0" "1.46" "1.47.0" "1.47" "1.48.0" "1.48" "1.49.0" "1.49"
        "1.50.0" "1.50" "1.51.0" "1.51" "1.52.0" "1.52" "1.53.0" "1.53" "1.54.0" "1.54"
        "1.55.0" "1.55" "1.56.0" "1.56" "1.57.0" "1.57" "1.58.0" "1.58" "1.59.0" "1.59"
        "1.60.0" "1.60" "1.61.0" "1.61" "1.62.0" "1.62" "1.63.0" "1.63" "1.64.0" "1.64"
        "1.65.0" "1.65" "1.66.0" "1.66" "1.67.0" "1.67" "1.68.0" "1.68" "1.69.0" "1.69"
        )
find_package(Boost "1.35" REQUIRED COMPONENTS ${BOOST_REQUIRED_COMPONENTS})

if (Boost_FOUND)
    message("Boost is found")
    include_directories(${Boost_INCLUDE_DIRS})
    link_directories(${Boost_LIBRARY_DIRS})
else (Boost_FOUND)
    message(FATAL_ERROR "Boost program-options required to build")
endif (Boost_FOUND)


add_executable(gps-sdr-sim gpssim.cpp gpssim.h)
target_link_libraries(gps-sdr-sim

    ${Boost_LIBRARIES}
    pthread
    uhd
    spdlog::spdlog
)