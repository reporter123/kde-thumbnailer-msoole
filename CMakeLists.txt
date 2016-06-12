project(kde-thumbnailer-msoole)
cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)

set(QT_MIN_VERSION "5.3.0")
find_package(ECM 1.0.0 REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR} ${CMAKE_SOURCE_DIR})

find_package(GObject REQUIRED)
find_package(GLIB2 REQUIRED)
find_package(LIBGSF REQUIRED)

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings)

find_package(Qt5 ${QT_MIN_VERSION} CONFIG REQUIRED COMPONENTS PrintSupport Gui)
find_package(KF5 REQUIRED COMPONENTS KIO Archive)

add_definitions(${QT_DEFINITIONS} ${KF5_DEFINITIONS} -DQT_USE_FAST_CONCATENATION -DQT_USE_FAST_OPERATOR_PLUS)

include_directories(${QT_INCLUDES} ${KF5_INCLUDES})

set(libkowmf_SRCS
    libkowmf/WmfAbstractBackend.cpp
    libkowmf/WmfDeviceContext.cpp
    libkowmf/WmfPainterBackend.cpp
    libkowmf/WmfParser.cpp
    libkowmf/WmfStack.cpp
    libkowmf/libkowmflogging.cpp
)

set(msoolethumbnail_SRCS msoolecreator.cpp ${libkowmf_SRCS})

add_library(msoolethumbnail ${msoolethumbnail_SRCS})
target_link_libraries(msoolethumbnail
    Qt5::Gui KF5::KIOWidgets
    ${GOBJECT_LIBRARIES}
    ${LIBGSF_LIBRARIES}
)

install(TARGETS msoolethumbnail DESTINATION ${PLUGIN_INSTALL_DIR})
install(FILES msoolethumbnail.desktop DESTINATION ${SERVICES_INSTALL_DIR})
