macro(use_cxx11)
if(CMAKE_VERSION VERSION_LESS "3.1.3")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
    set(CMAKE_CXX_STANDARD 11)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()
endmacro(use_cxx11)

use_cxx11()


add_executable(client client.cpp)

add_executable(srv srv.cpp)

add_library(section5 client.cpp conf.lua Config.hpp cpplang.hpp hello.hpp SalesData.hpp SpinLock.hpp srv.cpp Summary.hpp Zmq.hpp)
install (TARGETS section5 DESTINATION bin)

install (FILES client.cpp conf.lua Config.hpp cpplang.hpp hello.hpp SalesData.hpp SpinLock.hpp srv.cpp Summary.hpp Zmq.hpp DESTINATION include)
