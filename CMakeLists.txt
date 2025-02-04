cmake_minimum_required(VERSION 3.15)
project(SXUtil VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Collect source files
file(GLOB IMGUI_SRC imgui/*.cpp)
file(GLOB SRCS src/*.cpp src/*.c)
file(GLOB HEADERS headers/* include/*)

# Include only GLFW + OpenGL backends
set(IMGUI_BACKENDS 
    imgui/backends/imgui_impl_glfw.cpp 
    imgui/backends/imgui_impl_opengl3.cpp
)

# Find required libraries
find_package(OpenGL REQUIRED)
find_package(glfw3 REQUIRED)

# Include directories
if(APPLE)
    include_directories(include imgui imgui/backends /opt/homebrew/opt/openal-soft/include)
elseif(UNIX)
    include_directories(include imgui imgui/backends /usr/include/AL)
endif()

# Create executable
add_executable(main ${SRCS} ${IMGUI_SRC} ${IMGUI_BACKENDS} ${HEADERS})

# Link libraries
if(APPLE)
    target_link_libraries(main PRIVATE glfw OpenGL::GL /opt/homebrew/opt/openal-soft/lib/libopenal.dylib)
elseif(UNIX)
    target_link_libraries(main PRIVATE glfw OpenGL::GL /usr/lib/libopenal.so)
endif()
