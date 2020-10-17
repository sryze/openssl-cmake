project(openssl C)
cmake_minimum_required(VERSION 2.8)

set(OPENSSL_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../openssl-1.1.1h" CACHE STRING "OpenSSL source code directory")
set(OPENSSL_ARCH ${CMAKE_SYSTEM_PROCESSOR} CACHE STRING "Target architecture: X86 or AMD64")
set(OPENSSL_CONFIG "plain" CACHE STRING "OpenSSL build configuration: shared, minimal, or plain (default)")

if({OPENSSL_ARCH STREQUAL X86)
    set(_target VC-WIN32 no-asm)
else()
    set(_target VC-WIN64A-masm)
endif()

if("${OPENSSL_CONFIG}" STREQUAL shared)
    set(_shared no-makedepend)
elseif("${OPENSSL_CONFIG}" STREQUAL minimal)
    set(_shared no-shared no-dso no-makedepend no-aria no-async no-autoload-config no-blake2 no-bf no-camellia no-cast no-chacha no-cmac no-cms no-comp no-ct no-des no-dgram no-dh no-dsa no-dtls no-ec2m no-engine no-filenames no-gost no-idea no-mdc2 no-md4 no-multiblock no-nextprotoneg no-ocsp no-ocb no-poly1305 no-psk no-rc2 no-rc4 no-rmd160 no-seed no-siphash no-sm2 no-sm3 no-sm4 no-srp no-srtp no-ssl3 no-ssl3-method no-ts no-ui-console no-whirlpool no-asm -DOPENSSL_SMALL_FOOTPRINT)
else()
    set(_shared no-shared no-makedepend)
endif()

set(_configure_args --prefix=${CMAKE_INSTALL_PREFIX} --openssldir=${CMAKE_INSTALL_PREFIX} ${_target} ${_shared})
message("Configure arguments: ${_configure_args}")

find_package(Perl REQUIRED)
execute_process(
    COMMAND ${PERL_EXECUTABLE} ${OPENSSL_SOURCE_DIR}/Configure ${_configure_args}
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

add_custom_target(openssl
    ALL
    COMMAND nmake
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

add_custom_target(openssl_clean
    COMMAND nmake clean
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

add_custom_target(openssl_install
    COMMAND nmake install
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)