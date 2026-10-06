cmake_minimum_required(VERSION 3.22.1)
project(QualyxPhoneXR LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(phonexr_host SHARED
    ${CMAKE_ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c
    app/src/main/cpp/phonexr_host_wrapper.cpp
    app/src/main/cpp/phonexr_window.cpp
    app/src/main/cpp/phonexr_compositor.cpp
    app/src/main/cpp/phonexr_adreno_probe.cpp
)
target_include_directories(phonexr_host PRIVATE
    app/src/main/cpp
    ${CMAKE_ANDROID_NDK}/sources/android/native_app_glue
)
target_link_libraries(phonexr_host android log vulkan dl)

add_library(phonexr_probe SHARED
    ${CMAKE_ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c
    app/src/main/cpp/phonexr_probe.cpp
    app/src/main/cpp/phonexr_window.cpp
    app/src/main/cpp/phonexr_compositor.cpp
    app/src/main/cpp/phonexr_adreno_probe.cpp
)
target_include_directories(phonexr_probe PRIVATE
    app/src/main/cpp
    ${CMAKE_ANDROID_NDK}/sources/android/native_app_glue
)
target_link_libraries(phonexr_probe android log vulkan dl)

add_library(phonexr_runtime SHARED
    app/src/main/cpp/phonexr_runtime.cpp
    app/src/main/cpp/phonexr_compat.cpp
)
target_include_directories(phonexr_runtime PRIVATE app/src/main/cpp)
target_link_libraries(phonexr_runtime android log vulkan dl)
