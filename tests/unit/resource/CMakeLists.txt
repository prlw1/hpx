# Copyright (c) 2017 Hartmut Kaiser
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

set(tests
    named_pool_executor
    resource_partitioner_info
    shutdown_suspended_pus
    suspend_disabled
    suspend_pool
    suspend_pool_external
    suspend_runtime
    suspend_thread
    suspend_thread_external
    suspend_thread_timed
    used_pus
)

set(named_pool_executor_PARAMETERS THREADS_PER_LOCALITY 4)
set(resource_partitioner_info_PARAMETERS THREADS_PER_LOCALITY 4)
set(shutdown_suspended_pus_PARAMETERS THREADS_PER_LOCALITY 4)
set(suspend_disabled_PARAMETERS THREADS_PER_LOCALITY 4)
set(suspend_pool_PARAMETERS THREADS_PER_LOCALITY 4)
set(suspend_pool_external_PARAMETERS THREADS_PER_LOCALITY 4)
set(suspend_runtime_PARAMETERS THREADS_PER_LOCALITY 4)
set(suspend_thread_PARAMETERS THREADS_PER_LOCALITY 4)
set(suspend_thread_external_PARAMETERS THREADS_PER_LOCALITY 4)
set(suspend_thread_timed_PARAMETERS THREADS_PER_LOCALITY 4)
set(used_pus_PARAMETERS THREADS_PER_LOCALITY 4)

foreach(test ${tests})
  set(sources
      ${test}.cpp)

  source_group("Source Files" FILES ${sources})

  # add example executable
  add_hpx_executable(${test}_test
                     SOURCES ${sources}
                     ${${test}_FLAGS}
                     EXCLUDE_FROM_ALL
                     HPX_PREFIX ${HPX_BUILD_PREFIX}
                     FOLDER "Tests/Unit/ResourcePartitioner/")

  add_hpx_unit_test("resource" ${test} ${${test}_PARAMETERS})

  # add a custom target for this example
  add_hpx_pseudo_target(tests.unit.resource.${test})

  # make pseudo-targets depend on master pseudo-target
  add_hpx_pseudo_dependencies(tests.unit.resource
                              tests.unit.resource.${test})

  # add dependencies to pseudo-target
  add_hpx_pseudo_dependencies(tests.unit.resource.${test}
                              ${test}_test)
endforeach()

