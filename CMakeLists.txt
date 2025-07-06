cmake_minimum_required(VERSION 3.15)
project(vendored_openssl_crypto LANGUAGES C)

# Set default to static to avoid shared library complexity
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

# Disable all optional features
add_definitions(-DOPENSSL_NO_DEPRECATED -DOPENSSL_NO_SSL3 -DOPENSSL_NO_RC5 -DOPENSSL_NO_MD2)

# Include only required directories
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_SOURCE_DIR}
)

# Manually list only the crypto sources you use
set(CRYPTO_SOURCES
    crypto/sha/sha256.c
    crypto/evp/evp_enc.c
    crypto/evp/evp_lib.c
    crypto/evp/evp_cnf.c
    crypto/evp/digest.c
    crypto/evp/m_sha1.c
    crypto/evp/m_sha256.c
    crypto/evp/m_md5.c
    crypto/evp/encode.c
    crypto/evp/e_aes.c
    crypto/aes/aes_core.c
    crypto/aes/aes_cbc.c
    crypto/cmac/cmac.c
    crypto/mem.c
    crypto/o_init.c
    crypto/c_allc.c
    crypto/c_alld.c
    crypto/objects/obj_dat.c
    crypto/objects/obj_lib.c
    crypto/objects/obj_xref.c
    crypto/objects/obj_err.c
    crypto/err/err.c
)

# Create static library
add_library(vendored_openssl STATIC ${CRYPTO_SOURCES})

target_include_directories(vendored_openssl PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_compile_definitions(vendored_openssl PRIVATE OPENSSL_NO_DEPRECATED)