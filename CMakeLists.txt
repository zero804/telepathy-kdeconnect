project(telepathy-kdeconnect)

cmake_minimum_required(VERSION 2.8.12)

find_package(ECM 0.0.9 REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

find_package(Qt5 5.2 REQUIRED COMPONENTS Network DBus Xml)
find_package(TelepathyQt5 REQUIRED 0.9.5)
find_package(TelepathyQt5Service REQUIRED 0.9.7)

include(KDEInstallDirs)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(KDECMakeSettings)

add_executable(telepathy-kdeconnect
    main.cpp
    connection.cpp
    protocol.cpp
    textchannel.cpp
    kdeconnecttelepathyprotocolfactory.cpp
)

target_link_libraries(telepathy-kdeconnect
    Qt5::Core
    ${TELEPATHY_QT5_LIBRARIES}
    ${TELEPATHY_QT5_SERVICE_LIBRARIES}
)

install (
  TARGETS telepathy-kdeconnect
  DESTINATION ${KDE_INSTALL_LIBEXECDIR}
)

file(WRITE ${CMAKE_BINARY_DIR}/org.freedesktop.Telepathy.ConnectionManager.kdeconnect.service "[D-BUS Service]
Name=org.freedesktop.Telepathy.ConnectionManager.kdeconnect
Exec=${KDE_INSTALL_FULL_LIBEXECDIR}/telepathy-kdeconnect
")

install(FILES ${CMAKE_BINARY_DIR}/org.freedesktop.Telepathy.ConnectionManager.kdeconnect.service
        DESTINATION ${KDE_INSTALL_DBUSSERVICEDIR})

install(
    FILES kdeconnect.manager
    DESTINATION ${KDE_INSTALL_DATADIR}/telepathy/managers
)

install(
    FILES kdeconnect.profile
    DESTINATION ${KDE_INSTALL_DATADIR}/telepathy/profiles
)
