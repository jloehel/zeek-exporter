
cmake_minimum_required(VERSION 2.8)

project(ZeekPluginZeek_Exporter)

include(BroPlugin)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
add_subdirectory(3rdparty/prometheus-cpp)

include_directories(BEFORE ${prometheus-cpp_INCLUDE_DIR})

bro_plugin_begin(ESnet Zeek_Exporter)
bro_plugin_cc(src/Plugin.cc)
bro_plugin_bif(src/zeek_exporter.bif)
bro_plugin_link_library(prometheus-cpp::core prometheus-cpp::pull)
bro_plugin_dist_files(README CHANGES COPYING VERSION)
bro_plugin_end()

file(STRINGS "${CMAKE_CURRENT_SOURCE_DIR}/VERSION" VERSION LIMIT_COUNT 1)

if ("${PROJECT_SOURCE_DIR}" STREQUAL "${CMAKE_SOURCE_DIR}")
    # Allows building rpm/deb packages via "make package" in build dir.
    include(ConfigurePackaging)
    ConfigurePackaging(${VERSION})
endif ()
