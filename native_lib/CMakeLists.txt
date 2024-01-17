cmake_minimum_required(VERSION 3.16)

project(local_ai_app_gratis_library VERSION 0.0.1 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
 
message(STATUS "START COMPILE PACKAGE_TEMPLATE")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

set(local_ai_app_gratis_shared_dir "lib")
set(local_ai_app_gratis_exe_dir "bin")
set(prefix_shared_library "lib")
 

if (ANDROID)
 set(package_shared_extension "_${ANDROID_ARCH_NAME}.so")
 set(package_exe_extension "")
 set(package_name "local_ai_app_gratis_${ANDROID_ARCH_NAME}")
elseif (WIN32)
 set(package_shared_extension ".dll")
 set(package_exe_extension ".exe") 
 set(package_name "local_ai_app_gratis")
elseif (APPLE)
 set(package_shared_extension ".dylib")
 set(package_exe_extension "") 
 set(package_name "local_ai_app_gratis")
else()
 set(package_shared_extension ".so")
 set(package_exe_extension "") 
 set(package_name "local_ai_app_gratis")
endif()


add_library(local_ai_app_gratis_shared SHARED local_ai_app_gratis.cpp)
add_executable(local_ai_app_gratis_exe local_ai_app_gratis.cpp)

set_target_properties(local_ai_app_gratis_shared PROPERTIES 
  LIBRARY_OUTPUT_DIRECTORY "${local_ai_app_gratis_shared_dir}"
  PREFIX "${prefix_shared_library}"
  OUTPUT_NAME "${package_name}"
)

set_target_properties(local_ai_app_gratis_exe PROPERTIES 
  RUNTIME_OUTPUT_DIRECTORY "${local_ai_app_gratis_exe_dir}"
  OUTPUT_NAME "${package_name}"
)

if (WIN32)
 set_target_properties(local_ai_app_gratis_shared PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
)
endif (WIN32)
 
add_custom_command(
  TARGET local_ai_app_gratis_shared POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${local_ai_app_gratis_shared_dir}/${prefix_shared_library}local_ai_app_gratis${package_shared_extension}"
)

add_custom_command(
  TARGET local_ai_app_gratis_exe POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${local_ai_app_gratis_exe_dir}/local_ai_app_gratis${package_exe_extension}" 
)

 