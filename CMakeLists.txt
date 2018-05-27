cmake_minimum_required (VERSION 3.0)
project(PythonQt)
# project(PythonQt VERSION 3.2 LANGUAGES CXX DESCRIPTION "PythonQt: Embed Python within C++/Qt appplications")

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

set(PYTHONQT_SOURCES
  src/PythonQtStdDecorators.cpp
  src/PythonQt.cpp
  src/PythonQtClassInfo.cpp
  src/PythonQtImporter.cpp
  src/PythonQtObjectPtr.cpp
  src/PythonQtProperty.cpp
  src/PythonQtStdIn.cpp
  src/PythonQtStdOut.cpp
  src/PythonQtSignal.cpp
  src/PythonQtSlot.cpp
  src/PythonQtSlotDecorator.cpp
  src/PythonQtMisc.cpp
  src/PythonQtMethodInfo.cpp
  src/PythonQtConversion.cpp
  src/PythonQtSignalReceiver.cpp
  src/PythonQtInstanceWrapper.cpp
  src/PythonQtQFileImporter.cpp
  src/PythonQtClassWrapper.cpp
  src/PythonQtBoolResult.cpp
  src/gui/PythonQtScriptingConsole.cpp

  generated_cpp/com_trolltech_qt_core_builtin/com_trolltech_qt_core_builtin0.cpp
  generated_cpp/com_trolltech_qt_core_builtin/com_trolltech_qt_core_builtin_init.cpp
  generated_cpp/com_trolltech_qt_gui_builtin/com_trolltech_qt_gui_builtin0.cpp
  generated_cpp/com_trolltech_qt_gui_builtin/com_trolltech_qt_gui_builtin_init.cpp
)

set(PYTHONQT_HEADERS
  src/PythonQt.h
  src/PythonQtStdDecorators.h
  src/PythonQtClassInfo.h
  src/PythonQtImporter.h
  src/PythonQtObjectPtr.h
  src/PythonQtProperty.h
  src/PythonQtSignal.h
  src/PythonQtSlot.h
  src/PythonQtSlotDecorator.h
  src/PythonQtStdIn.h
  src/PythonQtStdOut.h
  src/PythonQtMisc.h
  src/PythonQtMethodInfo.h
  src/PythonQtImportFileInterface.h
  src/PythonQtConversion.h
  src/PythonQtSignalReceiver.h
  src/PythonQtInstanceWrapper.h
  src/PythonQtClassWrapper.h
  src/PythonQtCppWrapperFactory.h
  src/PythonQtQFileImporter.h
  src/PythonQtQFileImporter.h
  src/PythonQtVariants.h
  src/gui/PythonQtScriptingConsole.h
  src/PythonQtSystem.h
  src/PythonQtUtils.h
  src/PythonQtBoolResult.h

  generated_cpp/com_trolltech_qt_core_builtin/com_trolltech_qt_core_builtin0.h
  generated_cpp/com_trolltech_qt_gui_builtin/com_trolltech_qt_gui_builtin0.h
)

# Find Qt5 and link
find_package(Qt5 5.6 COMPONENTS Core Widgets REQUIRED)
include_directories(${Qt5Core_INCLUDE_DIRS} ${Qt5Widgets_INCLUDE_DIRS})
include_directories(${Qt5Core_PRIVATE_INCLUDE_DIRS})
add_definitions(${Qt5Core_DEFINITIONS} ${Qt5Widgets_DEFINITIONS})
list(APPEND QT_LIBRARIES ${Qt5Core_LIBRARIES} ${Qt5Widgets_LIBRARIES})

## Qt Libraries have to appear first, otherwise OS X deployment is confused.
list(APPEND LIBRARIES ${QT_LIBRARIES})

# Find Python and link
find_package(PythonInterp)
find_package(PythonLibs 3.5 REQUIRED)
add_definitions(
  -DPYTHONQT_USE_RELEASE_PYTHON_FALLBACK
  -DPYTHONQT_SUPPORT_NAME_PROPERTY
  )
include_directories(${PYTHON_INCLUDE_DIRS})
list(APPEND LIBRARIES ${PYTHON_LIBRARIES})

# Define PythonQt library
add_library(PythonQt STATIC ${PYTHONQT_SOURCES})
target_link_libraries(PythonQt ${LIBRARIES})
set_target_properties(PythonQt PROPERTIES DEFINE_SYMBOL PYTHONQT_EXPORTS)
# set_target_properties(PythonQt PROPERTIES VERSION ${PROJECT_VERSION})
set_target_properties(PythonQt PROPERTIES PUBLIC_HEADER src/PythonQt.h)
target_include_directories(PythonQt PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)
