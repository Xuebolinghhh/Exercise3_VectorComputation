cmake_minimum_required( VERSION 3.4.3 )
# Project name and programming languageproject( vectorcomputationsCXX )
# Gather files and store paths in CMake variables
file( GLOB LIBRARY_SOURCE_FILES library/src/*.cpp)
file( GLOB TEST_SOURCE_FILES library/test/*.cpp)
file( GLOB HEADER_FILES library/inc/*.h* )

# Specify the folders with header files, otherwise our includes are not found 
include_directories( external/catch )
include_directories( library/inc)

# Enables exporting all symbols to the dllon windows
set( CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON )

# When compiling with gccwe need to compile libraries with fPIC(stands for 
# position independent code).
if( CMAKE_COMPILER_IS_GNUCXX )
	set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC-std=c++11 -Wall -Wextra")
endif( CMAKE_COMPILER_IS_GNUCXX )

# Add compilation target for our library
add_library( vectorkernel SHARED ${LIBRARY_SOURCE_FILES} ${HEADER_FILES} )

# Add compilation target for our executables
add_executable( vectorkernel_testrunner ${TEST_SOURCE_FILES} ${HEADER_FILES} )
add_executable( main drivers/main.cpp ${HEADER_FILES} )

# Add link depenencies
target_link_libraries( vectorkernel_testrunner vectorkernel)
target_link_libraries( main vectorkernel)