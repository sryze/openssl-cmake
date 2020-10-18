OpenSSL - CMake
===============

Build OpenSSL with CMake on Windows:

1. Clone this repo
2. Run `mkdir build && cd build && cmake ../ -G "Visual Studio 16 2019 -DOPENSSL_SOURCE_DIR=path/to/openssl-x.y.z -DOPENSSL_ARCH=AMD64 -DOPENSSL_CONFIG=shared`
3. Open openssl.sln in Visual Studio and build it

Options:

* `OPENSSL_SOURCE_DIR` - Path to the root source directory of OpenSSL
* `OPENSSL_ARCH` - Target architecture: X86 or AMD64
* `OPENSSL_CONFIG` - OpenSSL build configuration: shared, minimal, or plain (default)
