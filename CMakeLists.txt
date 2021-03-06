cmake_minimum_required(VERSION 3.0)
project(lua-amf3)
find_package(PkgConfig)
pkg_search_module(LUA REQUIRED lua-5.1 lua5.1 lua51 lua>=5.1)
add_definitions(-ansi -pedantic -Wall -Wextra -Wshadow -Wformat -Wundef -Wwrite-strings -Wredundant-decls -Wno-uninitialized)
include_directories(${LUA_INCLUDE_DIRS})
link_directories(${LUA_LIBRARY_DIRS})
add_library(amf3 SHARED src/amf3.c src/amf3_encode.c src/amf3_decode.c)
target_link_libraries(amf3 ${LUA_LIBRARIES})
set_target_properties(amf3 PROPERTIES PREFIX "")
if(APPLE)
	set_target_properties(amf3 PROPERTIES SUFFIX ".so")
endif()
install(TARGETS amf3 LIBRARY DESTINATION lib/lua/5.1)
enable_testing()
find_program(LUA_COMMAND NAMES lua lua51 lua5.1 lua-5.1)
add_test(amf3 ${LUA_COMMAND} ${CMAKE_CURRENT_SOURCE_DIR}/test.lua)
