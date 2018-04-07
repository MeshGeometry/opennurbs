project(OpenNurbs)

ENABLE_LANGUAGE(C)

# Set minimum version
cmake_minimum_required (VERSION 2.8.6)

#make sure append this to debug
set(CMAKE_DEBUG_POSTFIX "_d")

#set (CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/CMake/Modules)

# Include Urho3D Cmake common module
#include (Urho3D-CMake-common)

add_subdirectory("./freetype263")

#define includes
include_directories("./")
include_directories("./zlib")
include_directories("./freetype263/include")

#create target
set (TARGET_NAME OpenNurbsLib)

#define source
file(GLOB OPENNURBS_SRC
    "./*.h"
    "./*.cpp")

file(GLOB ZLIB_SRC
    "./zlib/*.h"
    "./zlib/*.c")

list(APPEND SOURCE_FILES ${OPENNURBS_SRC} ${ZLIB_SRC})

# Setup target with resource copying
#setup_library ()
add_library(OpenNurbsLib ${SOURCE_FILES})
target_link_libraries(OpenNurbsLib FreeType)

if(MSVC)
	#remove the win32 define
	message(${CMAKE_CXX_FLAGS})
	string (REPLACE "/DWIN32" "/DWIN64" CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS}) 
	message(${CMAKE_CXX_FLAGS})
	add_definitions(-DUNICODE -D_UNICODE -D_LIB -DON_COMPILING_OPENNURBS)
endif()

set_target_properties(OpenNurbsLib PROPERTIES LINKER_LANGUAGE CXX)