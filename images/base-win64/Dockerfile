ARG GH_REPO=btbn/ffmpeg-builds
FROM docker.pkg.github.com/$GH_REPO/base:latest

ENV DEBIAN_FRONTEND noninteractive

RUN \
    apt-get -y install gcc-mingw-w64-x86-64 g++-mingw-w64-x86-64 gfortran-mingw-w64-x86-64 && \
    apt-get -y clean && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libstdc++*.dll* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libgcc_s* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/*.dll.a && \
    rm /usr/*-w64-mingw32/lib/*.dll.a && \
    mkdir /opt/ffbuild

RUN rustup target add x86_64-pc-windows-gnu

ADD toolchain.cmake /toolchain.cmake
ADD cross.meson /cross.meson

ENV FFBUILD_TARGET_FLAGS="--pkg-config=pkg-config --cross-prefix=x86_64-w64-mingw32- --arch=x86_64 --target-os=mingw32" \
    FFBUILD_TOOLCHAIN=x86_64-w64-mingw32 \
    FFBUILD_CROSS_PREFIX=x86_64-w64-mingw32- \
    FFBUILD_PREFIX=/opt/ffbuild \
    FFBUILD_CMAKE_TOOLCHAIN=/toolchain.cmake \
    PKG_CONFIG_LIBDIR=/opt/ffbuild/lib/pkgconfig \
    CFLAGS="-static-libgcc -static-libstdc++ -I/opt/ffbuild/include -O2 -pipe -D_FORTIFY_SOURCE=2 -fstack-protector-strong" \
    CXXFLAGS="-static-libgcc -static-libstdc++ -I/opt/ffbuild/include -O2 -pipe -D_FORTIFY_SOURCE=2 -fstack-protector-strong" \
    LDFLAGS="-static-libgcc -static-libstdc++ -L/opt/ffbuild/lib -O2 -pipe -fstack-protector-strong"
