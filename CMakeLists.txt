cmake_minimum_required(VERSION 3.1)
set(CMAKE_CXX_STANDARD 17)
project(ImApp)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")

# Source code
set(SOURCES
    main.cpp
)

# Header files
set(HEADERS
)

# Create executable
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})


#--------------------------------------------------------------------
# Add libraries
#--------------------------------------------------------------------
## GLFW
add_subdirectory(libs/glfw)
set(GLFW_INCLUDE_DIRS libs/glfw/include)
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
target_include_directories(${PROJECT_NAME}
PRIVATE
    src
    libs
    ${GLFW_INCLUDE_DIRS}
    ${OPENGL_INCLUDE_DIRS}
    ${GLEW_INCLUDE_DIRS}
)

# Link libraries
target_link_libraries(${PROJECT_NAME}
PRIVATE
    glfw
    OpenGL::GL
    GLEW::GLEW
    ${IMGUI_LIBRARIES}
)

# Add options
if (NOT MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE -Wno-narrowing)
endif()