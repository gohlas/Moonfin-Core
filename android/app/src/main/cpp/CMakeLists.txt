# Builds the Android JNI bridge and links the shared libretro host into it.
cmake_minimum_required(VERSION 3.22)
project(moonliz_libretro C)

# The portable host lives outside the android tree.
set(HOST_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../../../../../native/libretro_host)
add_subdirectory(${HOST_DIR} libretro_host_build)

add_library(moonliz_libretro SHARED native_game_jni.c)
target_include_directories(moonliz_libretro PRIVATE ${HOST_DIR})
# Keeps the per-frame blit optimized in debug builds, where the toolchain
# defaults to -O0 and the last -O wins.
target_compile_options(moonliz_libretro PRIVATE "$<$<CONFIG:Debug>:-O2>")
target_link_libraries(moonliz_libretro PRIVATE libretro_host android log)
