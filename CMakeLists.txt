cmake_minimum_required(VERSION 2.8)
project(libssh2-library)

option(LIBSSH2_DEBUG "Enable libssh2 debug" OFF)

# FIXME: Allow gcrypt
find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})

set(srcs
    # SRCS
    src/channel.c
    src/comp.c
    src/crypt.c
    src/hostkey.c
    src/kex.c
    src/mac.c
    src/misc.c
    src/packet.c
    src/publickey.c
    src/scp.c
    src/session.c
    src/sftp.c
    src/userauth.c
    src/transport.c
    src/version.c
    src/knownhost.c
    src/agent.c
    src/openssl.c
    src/libgcrypt.c
    src/pem.c
    src/keepalive.c
    src/global.c

    # Headers
    src/libssh2_priv.h
    src/openssl.h
    src/libgcrypt.h
    src/transport.h
    src/channel.h
    src/comp.h
    src/mac.h
    src/misc.h
    src/packet.h
    src/userauth.h
    src/session.h
    src/sftp.h
    src/crypto.h)

# FIXME: Most modern compiler has LONGLONG
# but it's better to check it...
add_definitions(-DHAVE_LONGLONG)
if(WIN32)
    include_directories(win32)
    add_definitions(-DLIBSSH2_WIN32)
elseif(APPLE)
    add_definitions(-DLIBSSH2_DARWIN)
endif()

if(LIBSSH2_DEBUG)
    add_definitions(-DLIBSSH2DEBUG)
endif()

include_directories(src include)

add_library(libssh2_STATIC STATIC
    ${srcs})

