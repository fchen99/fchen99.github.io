---
title: CMake windeployqt
tags: [cmake, qt]
date: 2023-11-27 10:30:00
---

vcpkg + CMake 构建 Qt 程序时，需要区分处理 `Debug`/`Release` 的 `windeployqt`。

```cmake
if (WIN32)
    get_target_property(QT_QMAKE_EXECUTABLE Qt6::qmake IMPORTED_LOCATION)
    get_filename_component(QT_DIR "${QT_QMAKE_EXECUTABLE}" DIRECTORY)

    if (CMAKE_BUILD_TYPE STREQUAL "Debug")
        set(WINDEPLOYQT_EXECUTABLE "${QT_DIR}/windeployqt.debug.bat")
    else ()
        set(WINDEPLOYQT_EXECUTABLE "${QT_DIR}/windeployqt.exe")
    endif ()

    add_custom_command(
        TARGET ${APP_NAME}
        POST_BUILD
        COMMAND "${WINDEPLOYQT_EXECUTABLE}"
                --dir "$<TARGET_FILE_DIR:${APP_NAME}>"
                "$<TARGET_FILE:${APP_NAME}>"
        COMMENT "Running windeployqt on ${APP_NAME}"
    )
endif ()
```
