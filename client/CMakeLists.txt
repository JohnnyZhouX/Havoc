cmake_minimum_required(VERSION 3.15)
project(Havoc)

# 基础设置
set(QT_VERSION 5)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# MSVC 特定设置
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /permissive-")
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_WINSOCK_DEPRECATED_NO_WARNINGS)

# Qt 设置
set(REQUIRED_LIBS Core Gui Widgets Network WebSockets Sql)
set(REQUIRED_LIBS_QUALIFIED Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Network Qt5::WebSockets Qt5::Sql)
set(APP_ICON_RESOURCE_WINDOWS "data/Havoc.qrc")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# 设置 Qt 路径（根据实际安装路径修改）
set(CMAKE_PREFIX_PATH "C:/Qt/5.15.2/msvc2019_64")

# Python 设置
set(Python_ADDITIONAL_VERSIONS 3.10)
find_package(Python3 COMPONENTS Interpreter Development REQUIRED)
set(PYTHONLIBS_VERSION_STRING ${Python3_VERSION})
set(PYTHON_INCLUDE_DIRS ${Python3_INCLUDE_DIRS})
set(PYTHON_LIBRARIES ${Python3_LIBRARIES})

# 包含目录
include_directories(
    include
    external/spdlog/include
    external/json/include
    external/toml
    ${PYTHON_INCLUDE_DIRS}
)

# 查找 Qt 包
find_package(Qt${QT_VERSION} COMPONENTS ${REQUIRED_LIBS} REQUIRED)

# 源文件设置
set(HAVOC_INCLUDE
    # Misc
    include/Havoc/Connector.hpp
    include/Havoc/DBManager/DBManager.hpp
    include/Havoc/DemonCmdDispatch.h
    include/Havoc/Packager.hpp
    include/Havoc/Havoc.hpp
    include/Havoc/Service.hpp
    include/Havoc/CmdLine.hpp

    # Python Api
    include/Havoc/PythonApi/PythonApi.h
    include/Havoc/PythonApi/Event.h
    include/Havoc/PythonApi/HavocUi.h
    include/Havoc/PythonApi/PyAgentClass.hpp
    include/Havoc/PythonApi/UI/PyDialogClass.hpp
    include/Havoc/PythonApi/UI/PyLoggerClass.hpp
    include/Havoc/PythonApi/UI/PyTreeClass.hpp
    include/Havoc/PythonApi/UI/PyWidgetClass.hpp

    # Dialogs
    include/UserInterface/Dialogs/Payload.hpp
    include/UserInterface/Dialogs/About.hpp
    include/UserInterface/Dialogs/Connect.hpp
    include/UserInterface/Dialogs/Listener.hpp

    # small widgets
    include/UserInterface/SmallWidgets/EventViewer.hpp

    # widgets
    include/UserInterface/Widgets/Chat.hpp
    include/UserInterface/Widgets/DemonInteracted.h
    include/UserInterface/Widgets/ListenerTable.hpp
    include/UserInterface/Widgets/ProcessList.hpp
    include/UserInterface/Widgets/PythonScript.hpp
    include/UserInterface/Widgets/SessionTable.hpp
    include/UserInterface/Widgets/SessionGraph.hpp
    include/UserInterface/Widgets/TeamserverTabSession.h
    include/UserInterface/Widgets/ScriptManager.h
    include/UserInterface/Widgets/LootWidget.h
    include/UserInterface/Widgets/FileBrowser.hpp
    include/UserInterface/Widgets/Teamserver.hpp
    include/UserInterface/Widgets/Store.hpp

    include/UserInterface/HavocUI.hpp
)

set(HAVOC_UI
    src/UserInterface/HavocUi.cc
    src/global.cc

    # Dialogs
    src/UserInterface/Dialogs/About.cc
    src/UserInterface/Dialogs/Connect.cc
    src/UserInterface/Dialogs/Listener.cc
    src/UserInterface/Dialogs/Payload.cc

    # Widgets
    src/UserInterface/Widgets/SessionTable.cc
    src/UserInterface/Widgets/SessionGraph.cc
    src/UserInterface/Widgets/Chat.cc
    src/UserInterface/Widgets/ListenersTable.cc
    src/UserInterface/Widgets/DemonInteracted.cc
    src/UserInterface/Widgets/TeamserverTabSession.cc
    src/UserInterface/Widgets/PythonScript.cc
    src/UserInterface/Widgets/ScriptManager.cc
    src/UserInterface/Widgets/LootWidget.cc
    src/UserInterface/Widgets/FileBrowser.cc
    src/UserInterface/Widgets/Teamserver.cc
    src/UserInterface/Widgets/Store.cc
    src/UserInterface/Widgets/ProcessList.cc

    # SmallWidgets
    src/UserInterface/SmallWidgets/EventViewer.cc
)

set(HAVOC_UTIL
    src/Util/ColorText.cpp
    src/Util/Base64.cpp
    src/Util/Base.cpp
)

set(HAVOC_SOURCE
    src/Main.cc
    src/Havoc/Packager.cc
    src/Havoc/Connector.cc
    src/Havoc/Service.cc
    src/Havoc/DBManger/DBManager.cc
    src/Havoc/DBManger/Teamserver.cc
    src/Havoc/DBManger/Scripts.cc
    src/Havoc/Demon/ConsoleInput.cc
    src/Havoc/Demon/CommandSend.cc
    src/Havoc/Demon/CommandOutput.cc
    src/Havoc/Demon/Commands.cc
    src/Havoc/PythonApi/PythonApi.cc
    src/Havoc/PythonApi/Havoc.cc
    src/Havoc/PythonApi/HavocUi.cc
    src/Havoc/PythonApi/UI/PyDialogClass.cc
    src/Havoc/PythonApi/UI/PyLoggerClass.cc
    src/Havoc/PythonApi/UI/PyTreeClass.cc
    src/Havoc/PythonApi/UI/PyWidgetClass.cc
    src/Havoc/PythonApi/PyDemonClass.cc
    src/Havoc/PythonApi/Event.cc
    src/Havoc/PythonApi/PyAgentClass.cc
    src/Havoc/Havoc.cc
    ${HAVOC_UI}
)

# 添加可执行文件
add_executable(${PROJECT_NAME} 
    ${HAVOC_INCLUDE} 
    ${HAVOC_SOURCE} 
    ${HAVOC_UTIL} 
    ${APP_ICON_RESOURCE_WINDOWS} 
    data/Havoc.rc
)

# 关闭 Qt 调试输出
add_definitions(-DQT_NO_DEBUG_OUTPUT)

# 链接库
target_link_libraries(${PROJECT_NAME}
    ${REQUIRED_LIBS_QUALIFIED}
    ${PYTHON_LIBRARIES}
)
