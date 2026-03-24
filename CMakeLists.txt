if(NOT DEFINED NSX_BOARD_FLAGS_TARGET)
    message(FATAL_ERROR "Board must define NSX_BOARD_FLAGS_TARGET before ns-uart is added.")
endif()
if(NOT DEFINED NSX_SOC_FAMILY)
    message(FATAL_ERROR "NSX_SOC_FAMILY must be defined before ns-uart is added.")
endif()

set(NSX_UART_ARCH_DIR "")
if(NSX_SOC_FAMILY STREQUAL "apollo3" OR NSX_SOC_FAMILY STREQUAL "apollo3p")
    set(NSX_UART_ARCH_DIR "apollo3")
elseif(NSX_SOC_FAMILY STREQUAL "apollo4l" OR NSX_SOC_FAMILY STREQUAL "apollo4p")
    set(NSX_UART_ARCH_DIR "apollo4")
elseif(NSX_SOC_FAMILY STREQUAL "apollo5b" OR NSX_SOC_FAMILY STREQUAL "apollo510" OR NSX_SOC_FAMILY STREQUAL "apollo510b")
    set(NSX_UART_ARCH_DIR "apollo5")
elseif(NSX_SOC_FAMILY STREQUAL "apollo330P")
    set(NSX_UART_ARCH_DIR "apollo330")
else()
    message(FATAL_ERROR "Unsupported NSX_SOC_FAMILY for ns-uart: ${NSX_SOC_FAMILY}")
endif()

add_library(nsx_uart STATIC
    ${CMAKE_CURRENT_LIST_DIR}/src/ns_uart.c
    ${CMAKE_CURRENT_LIST_DIR}/src/${NSX_UART_ARCH_DIR}/ns_uart.c
)
set_target_properties(nsx_uart PROPERTIES EXPORT_NAME uart)
add_library(nsx::uart ALIAS nsx_uart)

target_link_libraries(nsx_uart PUBLIC
    ${NSX_BOARD_FLAGS_TARGET}
    nsx_soc_hal
    nsx_core
    nsx_harness
    nsx_utils
)

target_include_directories(nsx_uart PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/includes-api>
    $<BUILD_INTERFACE:${NSX_ROOT}/modules/nsx-core/includes-api>
    $<BUILD_INTERFACE:${NSX_ROOT}/modules/nsx-harness/includes-api>
    $<BUILD_INTERFACE:${NSX_ROOT}/modules/nsx-utils/includes-api>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/nsx/modules/nsx-uart/includes-api>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/nsx/modules/nsx-core/includes-api>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/nsx/modules/nsx-harness/includes-api>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/nsx/modules/nsx-utils/includes-api>
)

install(DIRECTORY
    ${CMAKE_CURRENT_LIST_DIR}/includes-api
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/nsx/modules/nsx-uart
)

install(TARGETS
    nsx_uart
    EXPORT nsxTargets
)
