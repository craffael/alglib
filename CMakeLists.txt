cmake_minimum_required(VERSION 2.8.12)

cmake_policy(SET CMP0048 NEW)
project(alglib VERSION 3.18.0)

set(headers
  src/alglibinternal.h
  src/alglibmisc.h
  src/ap.h
  src/dataanalysis.h
  src/diffequations.h
  src/fasttransforms.h
  src/integration.h
  src/interpolation.h
  src/kernels_avx2.h
  src/kernels_fma.h
  src/kernels_sse2.h
  src/linalg.h
  src/optimization.h
  src/solvers.h
  src/specialfunctions.h
  src/statistics.h
  src/stdafx.h
)

set(sources
  src/alglibinternal.cpp
  src/alglibmisc.cpp
  src/ap.cpp
  src/dataanalysis.cpp
  src/diffequations.cpp
  src/fasttransforms.cpp
  src/integration.cpp
  src/interpolation.cpp
  src/kernels_avx2.cpp
  src/kernels_fma.cpp
  src/kernels_sse2.cpp
  src/linalg.cpp
  src/optimization.cpp
  src/solvers.cpp
  src/specialfunctions.cpp
  src/statistics.cpp
) 

add_library(alglib ${sources})
target_include_directories(alglib
  PUBLIC
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/src>
)

# Installation
set(config_install_dir "lib/cmake/alglib")
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(project_config "${generated_dir}/alglibConfig.cmake")
set(version_config "${generated_dir}/alglibConfigVersion.cmake")

install(TARGETS alglib EXPORT alglibTargets)
install(FILES ${headers} DESTINATION include/alglib)
install(EXPORT alglibTargets DESTINATION ${config_install_dir} NAMESPACE alglib::)

include(CMakePackageConfigHelpers)
configure_package_config_file(
  "Config.cmake.in"
  "${project_config}"
  INSTALL_DESTINATION "${config_install_dir}"
)
write_basic_package_version_file("${version_config}" COMPATIBILITY SameMajorVersion)
install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)
