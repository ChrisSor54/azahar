add_library(citra-android SHARED
    android_common/android_common.cpp
    android_common/android_common.h
    applets/mii_selector.cpp
    applets/mii_selector.h
    applets/swkbd.cpp
    applets/swkbd.h
    input_manager.cpp
    input_manager.h
    camera/ndk_camera.cpp
    camera/ndk_camera.h
    camera/still_image_camera.cpp
    camera/still_image_camera.h
    cheats/cheat.cpp
    cheats/cheat.h
    cheats/cheat_engine.cpp
    config.cpp
    config.h
    default_ini.h
    emu_window/emu_window.cpp
    emu_window/emu_window.h
    game_info.cpp
    id_cache.cpp
    id_cache.h
    native.cpp
    ndk_motion.cpp
    ndk_motion.h
    system_save_game.cpp
    native_log.cpp
    util.cpp
    util.h
)

target_link_libraries(citra-android PRIVATE audio_core citra_common citra_core input_common network)
target_link_libraries(citra-android PRIVATE android camera2ndk inih jnigraphics log mediandk yuv)

if (ENABLE_OPENGL)
    target_sources(citra-android PRIVATE
        emu_window/emu_window_gl.cpp
        emu_window/emu_window_gl.h
    )
    target_link_libraries(citra-android PRIVATE EGL glad)
endif()
if (ENABLE_VULKAN)
    target_sources(citra-android PRIVATE
        emu_window/emu_window_vk.cpp
        emu_window/emu_window_vk.h
    )
    if ("arm64" IN_LIST ARCHITECTURE)
        target_link_libraries(citra-android PRIVATE adrenotools)
    endif()
endif()

set(CPACK_PACKAGE_EXECUTABLES ${CPACK_PACKAGE_EXECUTABLES} citra-android)
