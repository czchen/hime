cmake_minimum_required(VERSION 3.7.1)
project(hime)

set(HIME_VERSION 0.9.10)

set(CMAKE_CXX_FLAGS "-O2 -Wall -Wextra ${CMAKE_CXX_FLAGS}")

include(GNUInstallDirs)

option(ENABLE_QT5 "Enable Qt5 input method module" On)

include_directories("${PROJECT_SOURCE_DIR}/src/im-client")

set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

if (ENABLE_QT5)
    find_package(Qt5 CONFIG REQUIRED Core Gui)

    include_directories(${Qt5Core_INCLUDE_DIRS})
    include_directories(${Qt5Gui_INCLUDE_DIRS})
    include_directories(${Qt5Gui_PRIVATE_INCLUDE_DIRS})

    add_definitions(${Qt5Core_DEFINITIONS})
    add_definitions(${Qt5Gui_DEFINITIONS})

    set(QT5_IMMODULE_SRCS
        src/qt5-im/hime-imcontext-qt.cpp
        src/qt5-im/hime-imcontext-qt.h
        src/qt5-im/hime-qt.cpp
        src/qt5-im/hime-qt.h
    )

    add_library(im-hime MODULE ${QT5_IMMODULE_SRCS})
endif()
