cmake_minimum_required(VERSION 3.19)
project(imgui VERSION 1.89.3 LANGUAGES C CXX)

set(CORE_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/imgui.h)
set(CORE_SOURCE_FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui_draw.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui_widgets.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui_tables.cpp)

if (IMGUI_SHOW_DEMO)
    message(STATUS "ImGui: Building demo")
    list(APPEND CORE_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/imgui_demo.cpp)
endif()

add_library(${PROJECT_NAME} ${CORE_SOURCE_FILES})
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

target_precompile_headers(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/pch.h)

if (IMGUI_OSX)
    message(STATUS "ImGui: Building for OSX")
    target_include_directories(${PROJECT_NAME} PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_osx.h)
    target_sources(${PROJECT_NAME} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_osx.mm)
    target_link_libraries(${PROJECT_NAME} PUBLIC "-framework GameController")
elseif (IMGUI_WIN32)
    message(STATUS "ImGui: Building for WIN32")
    target_sources(${PROJECT_NAME} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_win32.cpp)
    target_link_libraries(${PROJECT_NAME} PUBLIC d3d11.lib)
endif()

if (IMGUI_OPENGL3)
    message(STATUS "ImGui: Building OpenGL3")

    if (IMGUI_GLAD_LIB)
        message(STATUS "ImGui: Using GLAD")
        target_link_libraries(${PROJECT_NAME} PUBLIC ${IMGUI_GLAD_LIB})
        add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_CUSTOM)
    endif()

    target_include_directories(${PROJECT_NAME} PUBLIC 
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/mgui_impl_opengl3.h)
    target_sources(${PROJECT_NAME} PRIVATE 
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp)
endif()

if (IMGUI_GLFW_LIB)
    message(STATUS "ImGui: Building GLFW")
    target_include_directories(${PROJECT_NAME} PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.h)
    target_sources(${PROJECT_NAME} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp)
    target_link_libraries(${PROJECT_NAME} PUBLIC ${IMGUI_GLFW_LIB})
endif()

if (IMGUI_IMPL_METAL_CPP)
    message(STATUS "ImGui: Building Metal")
    target_include_directories(${PROJECT_NAME} PUBLIC 
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_metal.h)
    target_sources(${PROJECT_NAME} PRIVATE 
        ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_metal.cpp)
    target_link_libraries(${PROJECT_NAME} PUBLIC glfw)
endif()
