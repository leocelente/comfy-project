cmake_minimum_required(VERSION 3.12)
project(MyProject)

set(CMAKE_PREFIX_PATH ${CMAKE_BINARY_DIR})
set(CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR})

# CONFIG option is important so that CMake doesnt search for modules into the
# default modules directory
find_package(imgui CONFIG)
find_package(glfw3 CONFIG)
find_package(glew CONFIG)

# Language Server (compile_commands.json) ln -s build/compile_commands.json
# ./compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Conan
add_definitions("-std=c++2a")
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# Main Project
add_executable(
  my_project
  src/main.cpp src/module.cpp bindings/imgui_impl_glfw.cpp
  bindings/imgui_impl_opengl3.cpp bindings/imgui_impl_glfw.h
  bindings/imgui_impl_opengl3.h)

target_link_libraries(my_project ${CONAN_LIBS} imgui::imgui GLEW::glew_s
                      glfw::glfw)
target_compile_definitions(my_project PUBLIC IMGUI_IMPL_OPENGL_LOADER_GLEW)
target_include_directories(my_project PRIVATE "inc" "bindings")

# Main Tests
add_executable(my_project-tests tests/catch.cpp src/module.cpp tests/tests.cpp)
target_link_libraries(my_project-tests ${CONAN_LIBS})
target_include_directories(my_project-tests PRIVATE "inc")

enable_testing()
add_test(NAME "MyProject-Tests" COMMAND my_project-tests)
