cmake_minimum_required(VERSION 3.18)
if(NOT DEFINED DAEMON_TIMEOUT_STOP)
    set(DAEMON_TIMEOUT_STOP 120)
endif()
if(NOT DEFINED DAEMON_WAIT_RESTART)
    set(DAEMON_WAIT_RESTART 60)
endif()
set(EXTRAS "")
if (DEFINED DAEMON_ENVIRONMENTS)
    foreach(DAEMON_ENVIRONMENT ${DAEMON_ENVIRONMENTS})
        set(EXTRAS ${EXTRAS}Environment=${DAEMON_ENVIRONMENT}\n)
    endforeach(env ${})
endif()
configure_file("service.service" "${CMAKE_PROJECT_NAME}.service")
configure_file("postinst" "${CMAKE_BINARY_DIR}/daemon/postinst")
configure_file("prerm" "${CMAKE_BINARY_DIR}/daemon/prerm")
set(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA
    "${CMAKE_BINARY_DIR}/daemon/postinst;${CMAKE_BINARY_DIR}/daemon/prerm"
    PARENT_SCOPE
)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_PROJECT_NAME}.service"
    DESTINATION "/etc/systemd/system")
