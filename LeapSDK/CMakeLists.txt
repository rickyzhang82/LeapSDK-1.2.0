macro (SETUP_LIB_BY_CMAKE lib)
	
	find_package (${lib} REQUIRED)
	include_directories(${${lib}_INCLUDE_DIRS})
	list(APPEND PROJECT_LIBRARIES ${${lib}_LIBRARIES})
	list(APPEND PROJECT_LIBRARY_DIRS ${${lib}_LIBRARY_DIRS})

endmacro(SETUP_LIB_BY_CMAKE lib)

macro (SETUP_LIB_BY_PKGCONFIG lib)
	
	pkg_check_modules(${lib} REQUIRED ${lib})
	include_directories(${${lib}_INCLUDE_DIRS})
	list(APPEND PROJECT_LIBRARIES ${${lib}_LIBRARIES})
	list(APPEND PROJECT_LIBRARY_DIRS ${${lib}_LIBRARY_DIRS})

endmacro(SETUP_LIB_BY_PKGCONFIG lib)

cmake_minimum_required (VERSION 2.8)

project (LeapSDK-1.2.0)

find_package(PkgConfig REQUIRED)

#copy pkgconfig file

configure_file("${CMAKE_SOURCE_DIR}/libLeap.pc.cmake.in" "${CMAKE_BINARY_DIR}/libLeap.pc" @ONLY)

#install headers

install( DIRECTORY include/ 
         DESTINATION include/LeapSDK
         FILES_MATCHING PATTERN "*.[h|i]")

#install dynmaic lib
install( DIRECTORY lib/x64/ 
         DESTINATION lib/LeapSDK
         FILES_MATCHING PATTERN "*.so")

#install java and python wrapper from SWIG                      
install( FILES lib/Leap.py lib/LeapJava.jar
         DESTINATION lib/LeapSDK)

#install pkg-config file
install( FILES "${CMAKE_BINARY_DIR}/libLeap.pc"
         DESTINATION lib/pkgconfig)
