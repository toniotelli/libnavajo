set(hdr ${source_dir}/include PARENT_SCOPE)

add_library(libnavajo AuthPAM.cpp LocalRepository.cpp  LogFile.cpp  LogRecorder.cpp  LogStdOutput.cpp  LogSyslog.cpp  WebServer.cpp)
add_executable(navajoPrecompiler navajoPrecompiler.cpp)

install_targets(/bin navajoPrecompiler)
install_targets(/lib libnavajo)