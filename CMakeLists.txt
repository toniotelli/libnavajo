CMAKE_MINIMUM_REQUIRED(VERSION 3.0)
PROJECT(libnavajo)

# Definitions 
set (LIBNAVAJO_MVERSION "1")
set (LIBNAVAJO_VERSION "\\\"1.1.0\\\"")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DLINUX -DLIBNAVAJO_SOFTWARE_VERSION=${LIBNAVAJO_VERSION}")


set(source_dir ${CMAKE_CURRENT_SOURCE_DIR})
set(binary_dir ${CMAKE_CURRENT_BINARY_DIR})

set(BUILD_SHARED_LIBS ON)

# Required Package
find_package(PkgConfig)
if (PKG_CONFIG_FOUND)
	pkg_check_modules(OPENSSL REQUIRED openssl)
endif(PKG_CONFIG_FOUND)

# Directory Definition
set(src ${source_dir}/src)
set(hdr ${source_dir}/include)

# Includes
include_directories(
	${OPENSSL_INCLUDE_DIRS}
	${hdr})
# add_subdirectory(${src})

add_library(navajo ${src}/AuthPAM.cpp ${src}/LocalRepository.cpp  ${src}/LogFile.cpp  ${src}/LogRecorder.cpp  ${src}/LogStdOutput.cpp  ${src}/LogSyslog.cpp  ${src}/WebServer.cpp)
add_executable(navajoPrecompiler ${src}/navajoPrecompiler.cpp)

# Documentation
find_package(Doxygen)
if(DOXYGEN_FOUND)
	configure_file(${source_dir}/doxygen.conf ${binary_dir}/doxygen.conf @ONLY)
	add_custom_target(Docs ALL
		${DOXYGEN_EXECUTABLE} ${binary_dir}/doxygen.conf ${binary_dir}
	install(DIRECTORY ${binary_dir}/docs DESTINATION ${CMAKE_INSTALL_PREFIX}/)
	COMMENT "Generating API documentation with Doxygen" VERBATIM)
endif(DOXYGEN_FOUND)

# Install Targets
# install_files(/include/${PROJECT} ${hdr}/libnavajo)
install(DIRECTORY ${source_dir}/include/${PROJECT} DESTINATION include)
install_targets(/bin navajoPrecompiler)
install_targets(/lib navajo)
