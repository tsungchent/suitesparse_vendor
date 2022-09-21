cmake_minimum_required(VERSION 3.5)
project(suitesparse_vendor)

set(VERSION 1.5.0)

find_package(ament_cmake REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

if(WIN32)
    set(LAPACK_DIR ${CMAKE_CURRENT_BINARY_DIR}/suitesparse_src-prefix/src/suitesparse_src/lapack_windows/x64)
endif()

ExternalProject_Add(suitesparse_src
    GIT_REPOSITORY https://github.91chi.fun//https://github.com/jlblancoc/suitesparse-metis-for-windows.git
    GIT_TAG v${VERSION}
    UPDATE_COMMAND ""
    CONFIGURE_COMMAND cd ../suitesparse_src && cmake
      -DCMAKE_BUILD_TYPE=Release
      -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/suitesparse_install
      -DLAPACK_DIR=${LAPACK_DIR}
    BUILD_COMMAND cd ../suitesparse_src && cmake --build . --config Release
    INSTALL_COMMAND cd ../suitesparse_src && cmake --install .   
)

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/suitesparse_install/include/
        DESTINATION ${CMAKE_INSTALL_PREFIX}/include
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/suitesparse_install/lib/
        DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)

ament_package(CONFIG_EXTRAS "cmake/suitesparse-vendor-extras.cmake.in")
