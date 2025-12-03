cmake_minimum_required(VERSION 3.31)
project(McQueen)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_FLAGS_DEBUG "-ggdb -O0")

set(GLFW_SOURCES_DIR "${PROJECT_SOURCE_DIR}/glfw/")
add_subdirectory("${GLFW_SOURCES_DIR}")

set(GLAD_SOURCES_DIR "${PROJECT_SOURCE_DIR}/glad")
add_subdirectory("${GLAD_SOURCES_DIR}/cmake")

set(TCS_SOURCES_DIR "${PROJECT_SOURCE_DIR}/tinycsocket")
add_subdirectory("${TCS_SOURCES_DIR}")

set(IMGUI_SOURCES_DIR "${PROJECT_SOURCE_DIR}/imgui")
add_library(imgui
    "${IMGUI_SOURCES_DIR}/imgui.cpp"
    "${IMGUI_SOURCES_DIR}/imgui_demo.cpp"
    "${IMGUI_SOURCES_DIR}/imgui_draw.cpp"
    "${IMGUI_SOURCES_DIR}/imgui_tables.cpp"
    "${IMGUI_SOURCES_DIR}/imgui_widgets.cpp"
    "${IMGUI_SOURCES_DIR}/backends/imgui_impl_glfw.cpp"
    "${IMGUI_SOURCES_DIR}/backends/imgui_impl_opengl3.cpp"
)
target_include_directories(imgui PRIVATE ${IMGUI_SOURCES_DIR})
target_include_directories(imgui PRIVATE ${GLFW_SOURCES_DIR}/include)

glad_add_library(glad_gl_core_33 REPRODUCIBLE API gl:core=3.3)

add_executable(mcqueen
    src/main.cpp
)
target_include_directories(mcqueen PRIVATE ${IMGUI_SOURCES_DIR})
target_include_directories(mcqueen PRIVATE ${IMGUI_SOURCES_DIR}/backends)
set_property(TARGET mcqueen PROPERTY CXX_STANDARD 26)

target_compile_options(glfw PRIVATE -ggdb -O0)
target_compile_options(glad_gl_core_33 PRIVATE -ggdb -O0)
target_compile_options(imgui PRIVATE -ggdb -O0)
target_compile_options(tinycsocket PRIVATE -ggdb -O0)

# Add glad to your project
target_link_libraries(mcqueen
    PUBLIC
    glad_gl_core_33
    glfw
    tinycsocket
    imgui
)
