cmake_minimum_required(VERSION 3.19)
project(imgui VERSION 1.89.3 LANGUAGES CXX)

add_library(${PROJECT_NAME} 
    imgui_draw.cpp
    imgui_widgets.cpp
    imgui_tables.cpp
    imgui.cpp)

target_include_directories(${PROJECT_NAME} PUBLIC imgui.h)
