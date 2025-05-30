# ------------------------------------------------------------------------------
# Build Tests
# ------------------------------------------------------------------------------

add_executable(test_performance_exec
        ${TEST_MAIN}/main.cc
        test_init.cc
        test_latency.cc)
add_dependencies(test_performance_exec hermes)
target_link_libraries(test_performance_exec
        hermes Catch2::Catch2 MPI::MPI_CXX)

# add_executable(test_performance_exec
# ${TEST_MAIN}/main.cc
# test_init.cc
# test_latency.cc
# test_zmq.cc
# )
# add_dependencies(test_performance_exec
# ${Hermes_CLIENT_DEPS} hermes)
# target_link_libraries(test_performance_exec
# ${Hermes_CLIENT_DEPS} Catch2::Catch2
# MPI::MPI_CXX ${ZMQ_LIBRARIES})
add_executable(hermes_api_bench
        hermes_api_bench.cc)
add_dependencies(hermes_api_bench
        ${Hermes_CLIENT_DEPS})
target_link_libraries(hermes_api_bench
        ${Hermes_CLIENT_DEPS}
        Catch2::Catch2 MPI::MPI_CXX)

# ------------------------------------------------------------------------------
# Test Cases
# ------------------------------------------------------------------------------

# STRING TESTS
# add_test(NAME test_performance COMMAND
# ${CMAKE_BINARY_DIR}/bin/test_performance_exec "TestIpc")

# ------------------------------------------------------------------------------
# Install Targets
# ------------------------------------------------------------------------------
install(TARGETS
        test_performance_exec
        hermes_api_bench
        LIBRARY DESTINATION ${HERMES_INSTALL_LIB_DIR}
        ARCHIVE DESTINATION ${HERMES_INSTALL_LIB_DIR}
        RUNTIME DESTINATION ${HERMES_INSTALL_BIN_DIR})

# -----------------------------------------------------------------------------
# Coverage
# -----------------------------------------------------------------------------
if(HERMES_ENABLE_COVERAGE)
        set_coverage_flags(test_performance_exec)
        set_coverage_flags(hermes_api_bench)
endif()
