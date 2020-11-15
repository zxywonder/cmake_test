cmake_minimum_required (VERSION 3.3.0)

project (exe_demo)

#CMAKE_SOURCE_DIR 源代码所在路径 PROJECT_SOURCE_DIR
message(status ${PROJECT_SOURCE_DIR}  ${CMAKE_SOURCE_DIR})

#设置生成位置 可以使用_DEBUG 和 _RELEASE 后缀 区分debug和release
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)

message(status ${CMAKE_DEBUG_POSTFIX} "" ${EXECUTABLE_OUTPUT_PATH})


#设置生成名称


#set_target_properties(exe_demo PROPERTIES TARGET_NAME "Exe_Demo")

#CMAKE_DEBUG_POSTFIX debug 后缀 CMAKE_RELEASE_POSTFIX
set(CMAKE_DEBUG_POSTFIX "_d") #设置库文件后缀
set_target_properties(${TARGET_NAME} PROPERTIES DEBUG_POSTFIX "_d")  #设置可执行文件的后缀

#使用预编译头文件


#源文件
set(OTHER_FILES "")
file(GLOB SOURCE_FILES *.cpp)
file(GLOB_RECURSE HEADER_FILES "*.h" "*.hpp" "*.inc")

#筛选器
source_group("Header Files" FILES ${HEADER_FILES})
source_group("Source Files" FILES ${SOURCE_FILES})
source_group("" FILES ${OTHER_FILES})

#添加包含路径
include_directories($ENV{QT4_DIR}/include)

#添加库文件路径
Link_directories($ENV{ThirdPart}/Qt/4.7.0_x64/lib)

add_executable(exe_demo ${SOURCE_FILES} ${HEADER_FILES})

set(pch_header "stdafx.h")
set(pch_source_path "stdafx.cpp")
	set_target_properties(exe_demo PROPERTIES COMPILE_FLAGS "/Yu${pch_header}" "/FI${pch_header}" "/Fp${EXECUTABLE_OUTPUT_PATH}/pch/exe_demo/${pch_header}.pch")
    set_source_files_properties("${pch_source_path}" PROPERTIES COMPILE_FLAGS "/Yc${pch_header}")
    #set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "Fp${EXECUTABLE_OUTPUT_PATH}/pch/$exe_demo/${pch_header}.pch")



#添加附加依赖项 区分debug 和 release
target_link_libraries(exe_demo debug QtCored4.lib)
target_link_libraries(exe_demo optimized  QtCore4.lib)
