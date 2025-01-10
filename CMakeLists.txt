# some config options were copied from this guide: https://mprtmma.medium.com/game-dev-journey-1-7ad1a8181452
cmake_minimum_required(VERSION 3.15)
project(conan_raylib)

find_package(raylib REQUIRED)

# copy resource files, reference: https://github.com/raysan5/raylib/blob/master/examples/CMakeLists.txt
file(GLOB resources src/resources/*)
list(APPEND example_resources ${resources})
file(COPY ${example_resources} DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/resources) # reference: https://stackoverflow.com/q/34799916

add_executable(${PROJECT_NAME} src/main.c)

target_link_libraries(${PROJECT_NAME} PUBLIC raylib)
target_include_directories(${PROJECT_NAME} PUBLIC ${raylib_INCLUDE_DIRS})

#######################
# MSVC config options
#######################
if(MSVC)
	target_link_libraries(${PROJECT_NAME} PUBLIC wsock32 ws2_32)
	set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /SUBSYSTEM:WINDOWS /ENTRY:mainCRTStartup")
endif()