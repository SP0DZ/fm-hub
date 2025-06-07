#include_directories(trx svxlink)

#option(WITH_SYSTEMD "Set to ON to include systemd support" OFF)
#message(STATUS "With Systemd = ${WITH_SYSTEMD}")
#if(WITH_SYSTEMD)
#  add_subdirectory(systemd)
#endif(WITH_SYSTEMD)

#add_subdirectory(digital)
#add_subdirectory(trx)
#add_subdirectory(svxlink)
#add_subdirectory(modules)
#add_subdirectory(scripts)
#add_subdirectory(remotetrx)
#add_subdirectory(reflector)
#add_subdirectory(siglevdetcal)
#add_subdirectory(devcal)
#add_subdirectory(contrib)

# Find the popt library
find_package(Popt REQUIRED)
set(LIBS ${LIBS} ${POPT_LIBRARIES})
include_directories(${POPT_INCLUDE_DIRS})
add_definitions(${POPT_DEFINITIONS})

# Find the GCrypt library
#find_package(GCrypt REQUIRED)
#set(LIBS ${LIBS} ${GCRYPT_LIBRARIES})
#include_directories(${GCRYPT_INCLUDE_DIRS})
#add_definitions(${GCRYPT_DEFINITIONS})

# Find jsoncpp library
#find_package(jsoncpp REQUIRED)
#get_target_property(JSONCPP_INC_PATH jsoncpp_lib INTERFACE_INCLUDE_DIRECTORIES)
#include_directories(${JSONCPP_INC_PATH})
#set(LIBS ${LIBS} jsoncpp_lib)
pkg_check_modules (JSONCPP REQUIRED jsoncpp)
include_directories(${JSONCPP_INCLUDE_DIRS})
set(LIBS ${LIBS} ${JSONCPP_LIBRARIES})

# Add project libraries
set(LIBS asynccpp asyncaudio asynccore svxmisc ${LIBS})

# Build the executable
add_executable(fmhub
  fmhub.cpp Reflector.cpp ReflectorClient.cpp TGHandler.cpp
)
target_link_libraries(fmhub ${LIBS})
set_target_properties(fmhub PROPERTIES
  RUNTIME_OUTPUT_DIRECTORY ${RUNTIME_OUTPUT_DIRECTORY}
)

# Generate config file with correct paths
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/fmhub.conf.in
  ${CMAKE_CURRENT_BINARY_DIR}/fmhub.conf
  @ONLY
  )
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/ca-hook.py.in
  ${CMAKE_CURRENT_BINARY_DIR}/ca-hook.py
  @ONLY
  )

# Install targets
install(TARGETS fmhub DESTINATION ${BIN_INSTALL_DIR})
install_if_not_exists(${CMAKE_CURRENT_BINARY_DIR}/fmhub.conf
  ${SVX_SYSCONF_INSTALL_DIR}
  )
install_if_not_exists(ca-hook.yaml ${SVX_SYSCONF_INSTALL_DIR})
install(PROGRAMS fmhub-status
  DESTINATION ${BIN_INSTALL_DIR}
  )
install(PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/ca-hook.py
  DESTINATION ${SVX_SHARE_INSTALL_DIR}
  )
