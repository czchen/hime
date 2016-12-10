cmake_minimum_required(VERSION 3.7.1)
project(hime)

set(HIME_VERSION 0.9.10)

set(CMAKE_CXX_FLAGS "-O2 -Wall -Wextra ${CMAKE_CXX_FLAGS}")

include(GNUInstallDirs)

option(ENABLE_CHEWING "Enable chewing input method" On)
option(ENABLE_QT4 "Enable Qt5 input method module" On)
option(ENABLE_QT5 "Enable Qt5 input method module" On)

include_directories("${PROJECT_SOURCE_DIR}/src/im-client")

set(CMAKE_INCLUDE_CURRENT_DIR ON)

if (ENABLE_CHEWING)
    find_library(CHEWING chewing)

    if (NOT CHEWING)
        message(SEND_ERROR "cannot find chewing")
    endif()

    # FIXME: compile src/modules/chewing*
endif()

if (ENABLE_QT4)
    find_package(Qt4 REQUIRED)

    qt4_wrap_cpp(hime-qt4 QT4_IMMODULE_MOC_SRCS
        src/qt4-im/hime-qt.h
    )

    set(QT4_IMMODULE_SRCS
        src/qt4-im/hime-imcontext-qt.cpp
        src/qt4-im/hime-qt.cpp
        src/qt4-im/im-hime-qt.cpp
    )

    add_library(hime-qt4 MODULE ${QT4_IMMODULE_SRCS})
    target_include_directories(hime-qt4 PRIVATE
        ${QT_INCLUDES}
    )
endif()

if (ENABLE_QT5)
    find_package(Qt5 CONFIG REQUIRED Core Gui)

    qt5_wrap_cpp(hime-qt5 QT5_IMMODULE_MOC_SRCS
        src/qt5-im/hime-imcontext-qt.h
        src/qt5-im/hime-qt.h
    )

    set(QT5_IMMODULE_SRCS
        src/qt5-im/hime-imcontext-qt.cpp
        src/qt5-im/hime-qt.cpp
    )

    add_library(hime-qt5 MODULE ${QT5_IMMODULE_SRCS})
    target_include_directories(hime-qt5 PRIVATE
        ${Qt5Core_INCLUDE_DIRS}
        ${Qt5Gui_INCLUDE_DIRS}
        ${Qt5Gui_PRIVATE_INCLUDE_DIRS}
    )
endif()
