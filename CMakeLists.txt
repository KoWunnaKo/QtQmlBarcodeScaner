cmake_minimum_required(VERSION 3.5)

project(QtQmlBarcodeScaner LANGUAGES CXX)


set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# QtCreator supports the following variables for Android, which are identical to qmake Android variables.
# Check http://doc.qt.io/qt-5/deployment-android.html for more information.
# They need to be set before the find_package(Qt5 ...) call.

#if(ANDROID)
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
#    if (ANDROID_ABI STREQUAL "armeabi-v7a")
#        set(ANDROID_EXTRA_LIBS
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libcrypto.so
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libssl.so)
#    endif()
#endif()

find_package(Qt5 COMPONENTS Core Quick REQUIRED)

set(QZXING_MULTIMEDIA ON)
set(QZXING_USE_QML ON)
add_definitions(-DQZXING_QML)
add_definitions(-DQZXING_MULTIMEDIA)
add_definitions(-DENABLE_ENCODER_GENERIC)

add_definitions(-DENABLE_DECODER_QR_CODE)
#add_definitions(-DENABLE_DECODER_AZTEC)
add_definitions(-DENABLE_DECODER_1D_BARCODES)
add_definitions(-DNO_ICONV)

add_subdirectory(qzxing/src)


if(ANDROID)
    add_library(QtQmlBarcodeScaner SHARED
      main.cpp
      qml.qrc
    )
else()
    add_executable(QtQmlBarcodeScaner
      main.cpp
      qml.qrc
    )
endif()

target_compile_definitions(QtQmlBarcodeScaner
  PRIVATE $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>)
target_link_libraries(QtQmlBarcodeScaner
  PRIVATE Qt5::Core Qt5::Quick qzxing)

