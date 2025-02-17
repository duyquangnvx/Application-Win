cmake_minimum_required(VERSION 3.1)
set(CMAKE_CXX_STANDARD 17)
project(ImApp)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")

# Source code
set(SOURCES
    main.cpp
    src/Application.cpp
)

# Header files
set(HEADERS
    src/Application.h
)

# Create executable
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})


#--------------------------------------------------------------------
# Add libraries
#--------------------------------------------------------------------
## GLFW
add_subdirectory(libs/glfw)
set(GLFW_INCLUDE_DIRS libs/glfw/include)
set(OPENGL_LIBRARIES glfw)
# if(WIN32)
#     find_package(GLFW3 REQUIRED)
# else()
#     find_package(PkgConfig REQUIRED)
#     pkg_check_modules(GLFW REQUIRED glfw3)
# endif()

## OpenGL
find_package(OpenGL REQUIRED)

## GLEW
find_package(GLEW REQUIRED)

## ImGui
add_subdirectory(libs/imgui)


#--------------------------------------------------------------------
# Add include directories
#--------------------------------------------------------------------
target_include_directories(${PROJECT_NAME} PRIVATE
    src
    libs
    ${GLFW_INCLUDE_DIRS}
    ${OPENGL_INCLUDE_DIRS}
    ${GLEW_INCLUDE_DIRS}
    ${IMGUI_INCLUDE_DIRS}
)

# Link libraries
target_link_libraries(${PROJECT_NAME} 
    ${GLFW_LIBRARIES}
    ${OPENGL_LIBRARIES}
    ${IMGUI_LIBRARIES}
)

#--------------------------------------------------------------------
# Config install
#--------------------------------------------------------------------
set(BINARY_INST_DESTINATION "bin")
set(RESOURCE_INST_DESTINATION "share/my_shader_editor")
install(PROGRAMS bin/MyShaderEditor DESTINATION "${BINARY_INST_DESTINATION}" RENAME my_shader_editor)

# Add options
if (NOT MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE -Wno-narrowing)
endif()