file(GLOB MODULE_SOURCES "*.c")

add_library(${module_name} SHARED ${MODULE_SOURCES})

find_package(OpenSSL REQUIRED)
find_package(PkgConfig REQUIRED)

pkg_check_modules(nghttp2 REQUIRED IMPORTED_TARGET libnghttp2)
add_library(nghttp2::nghttp2 ALIAS PkgConfig::nghttp2)

pkg_check_modules(event REQUIRED IMPORTED_TARGET libevent libevent_openssl)
add_library(event::event ALIAS PkgConfig::event)

target_link_libraries(${module_name} PRIVATE nghttp2::nghttp2 event::event
                                             OpenSSL::SSL OpenSSL::Crypto)
