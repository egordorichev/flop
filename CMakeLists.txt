cmake_minimum_required(VERSION 3.16)
project(flop C)
set(CMAKE_C_STANDARD 99)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/")
find_package(Lit REQUIRED)

include_directories(include/ ${LIT_INCLUDE_DIRS})
add_subdirectory(util/embed-file/)

add_custom_command(
	OUTPUT src/lit_flop.c
	COMMAND embedfile src/ lit_flop include/flop.lit
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
	DEPENDS include/flop.lit)

add_executable(flop src/main.c src/lit_flop.c)
set_target_properties(flop PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/dist")
target_link_libraries(flop ${LIT_LIBRARIES} m dl)
