cmake_minimum_required(VERSION 3.5)
project(obs-v4l2sink)


include(external/FindLibObs.cmake)
find_package(LibObs REQUIRED)

set (CMAKE_CXX_STANDARD 11)
set(CMAKE_PREFIX_PATH "${QTDIR}")
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

find_package(Qt5Core REQUIRED)
find_package(Qt5Widgets REQUIRED)

file(GLOB_RECURSE FRONTEND_API_HEADER_PATH "/usr/*/*/obs-frontend-api.h")
get_filename_component(FRONTEND_API_HEADER_DIR "${FRONTEND_API_HEADER_PATH}" DIRECTORY)

set(virtualoutput_SOURCES
	src/v4l2sink.cpp
	src/v4l2sinkproperties.cpp)

set(virtualoutput_HEADERS
	src/v4l2sink.h
	src/v4l2sinkproperties.h)

include_directories("${FRONTEND_API_HEADER_DIR}")

add_library(v4l2sink MODULE
	${virtualoutput_SOURCES}
	${virtualoutput_HEADERS})
	
target_link_libraries(v4l2sink
	libobs
	Qt5::Core
	Qt5::Widgets)

if(ARCH EQUAL 64)
	set(ARCH_NAME "x86_64")
else()
	set(ARCH_NAME "i686")
endif()

set_target_properties(v4l2sink PROPERTIES PREFIX "")

install(TARGETS v4l2sink
	LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/obs-plugins)

install(DIRECTORY locale/
	DESTINATION "${CMAKE_INSTALL_PREFIX}/share/obs/obs-plugins/v4l2sink/locale")

