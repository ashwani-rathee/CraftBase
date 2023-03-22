cmake_minimum_required(VERSION 3.8.2) # 3.8.0 is minimum version that allow Linux remote building and debugging

project(libcraftbase)

# Remove trailing '\' and '/' at the end of the path, so can reliably compare
STRING(REGEX REPLACE "[/\\]+$" "" CMAKE_SOURCE_DIR ${CMAKE_SOURCE_DIR})
STRING(REGEX REPLACE "[/\\]+$" "" CMAKE_CURRENT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR})

# When compiling DLib as stand alone library set to C++11,
# when library is part of other project, respect parent project settings.
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
	# Enforce c++11 standard.
	set (CMAKE_CXX_STANDARD 11)
endif()

# Notification when too low version of C++ is set
if(NOT DEFINED CMAKE_CXX_STANDARD OR CMAKE_CXX_STANDARD LESS 11)
	message(FATAL_ERROR "ERROR: CMAKE_CXX_STANDARD is not set or it is too low. Minimum C++ v11 is required to compile \"DLib\" library.")
endif()

   
FILE(GLOB SOURCE_FILES src/*.c)
FILE(GLOB HEADER_FILES src/*.h)

set(MODULE_FILES
	${SOURCE_FILES}
	${HEADER_FILES}
)


add_library(libcraftbase SHARED ${MODULE_FILES})


#include(GenerateExportHeader)
#generate_export_header(DLib)

message(HEADERFILES="${HEADER_FILES}")
message(CMAKE_CURRENT_SOURCE_DIR="${CMAKE_CURRENT_SOURCE_DIR}")
# Make sure the compiler can find include files for our Hello library
# when other libraries or executables link to Hello
target_include_directories(libcraftbase PUBLIC ${HEADER_FILES})

message(LIBDIR="${libdir}")
# List files that install and/or package should provide.
# Each library should know what it want to distribute,
# which files are internal or intermediate and which are public library export.
install(TARGETS libcraftbase DESTINATION ${libdir})
install(FILES ${HEADER_FILES} DESTINATION ${includedir}/)
