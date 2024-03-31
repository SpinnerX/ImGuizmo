cmake_minimum_required(VERSION 3.15)

set(PROJECT_NAME ImGuizmo)
set(CMAKE_CXX_STANDARD 20)
project(${PROJECT_NAME} VERSION 1.0)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_CXX_VISIBILITY_PRESET hidden)
set(CMAKE_VISIBILITY_INLINES_HIDDEN ON)

option(ImGuizmo_BUILD_UNIT_TESTS "Build the ImGuizmo unit tests" ON)
option(ImGuizmo_BUILD_TESTBED "Build the ImGuizmo testbed" ON)
option(ImGuizmo_BUILD_DOCS "Build the ImGuizmo documentation" OFF)
option(ImGuizmo_USER_SETTINGS "Override ImGuizmo settings with ImGuizmoUserSettings.h" OFF)

option(BUILD_SHARED_LIBS "Build ImGuizmo as a shared library" OFF)

include(GNUInstallDirs)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)


if(APPLE)
# Finding opengl, glfw, glew, glut, and possibly freetype and even freeglut
find_package(glfw3 3.4 REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)

find_package(GLUT REQUIRED)
find_package(FREEGLUT REQUIRED)

find_package(fmt REQUIRED)
find_package(spdlog REQUIRED)
find_package(glad REQUIRED)
find_package(imgui REQUIRED)
endif(APPLE)

if(UNIX AND NOT APPLE)
# Finding opengl, glfw, glew, glut, and possibly freetype and even freeglut
find_package(glfw3 3.4 REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)

find_package(fmt REQUIRED)
find_package(spdlog REQUIRED)
find_package(glad REQUIRED)
find_package(imgui REQUIRED)
endif(UNIX AND NOT APPLE)


add_subdirectory(src)


if(APPLE)

    # This is making sure we find the SDL libraries on mac
    # include_directories(${SDL2_INCLUDE_DIRS})


    # target_link_libraries(${PROJECT_NAME} pthread)
    # Linking the libraries for Cocoa, OpenGL, IOKit, and glfw
    target_link_libraries(${PROJECT_NAME}
      "-framework Cocoa"
      "-framework OpenGL"
      "-framework IOKit"
      glfw
      ${OPENGL_gl_LIBRARY}
      GLEW::GLEW
      ${GLUT_LIBRARIES} # NOTE: If you want to use freeglut, then you need this to link freeglut to get working
      glad::glad
      imgui::imgui
    )
endif(APPLE)

if(WIN32)
target_link_libraries(${PROJECT_NAME} imgui)
endif(WIN32)

target_include_directories(ImGuizmo
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
  PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
)

install(
  DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/ImGuizmo"
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)