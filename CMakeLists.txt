cmake_minimum_required(VERSION 2.8.3)
project(trakstar)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(catkin REQUIRED COMPONENTS geometry_msgs tf roscpp genmsg)

# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})

find_package(PkgConfig)
pkg_check_modules(PC_LIBUSB REQUIRED libusb-1.0)

add_message_files(
  FILES
  TrakstarMsg.msg
)

## Generate added messages and services with any dependencies listed here
 generate_messages(
   DEPENDENCIES
   geometry_msgs
   std_msgs
 )

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES trakstar 
    CATKIN_DEPENDS geometry_msgs tf
)
include_directories(include 
                    ${catkin_INCLUDE_DIRS} 
                    )

link_directories(${catkin_LIBRARY_DIRS})


add_subdirectory(src)
add_subdirectory(tools)
add_subdirectory(nodes)

install(DIRECTORY include/ DESTINATION include)

install(DIRECTORY launch DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY config DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})


# Install udev rules
option(INSTALL_TRAKSTAR_UDEV_RULES "Install udev rules for Trakstar" ON)
IF (INSTALL_TRAKSTAR_UDEV_RULES)
  INSTALL (
    FILES config/99-trakstar.rules
    DESTINATION "/etc/udev/rules.d"
    COMPONENT "udev"
    )
else (INSTALL_TRAKSTAR_UDEV_RULES)
  message (STATUS "Udev rules for trakstar not being installed, install them with -DINSTALL_TRAKSTAR_UDEV_RULES=ON")
endif (INSTALL_TRAKSTAR_UDEV_RULES)


