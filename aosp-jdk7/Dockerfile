FROM debian:jessie as builder

RUN apt update \
    && apt install --yes \
        wget \
        g++-multilib \
        ca-certificates \
        build-essential \
        --no-install-recommends

COPY ncurses-5.9-gcc-5.patch ncurses.patch
RUN wget https://github.com/mirror/ncurses/archive/v5.9.tar.gz \
    && tar xf v5.9.tar.gz \
    && cd ncurses-5.9 \
    && patch -p1 < ../ncurses.patch \
    && ./configure --prefix /opt/x86_64 --with-shared --with-termlib \
    && make -j 8 \
    && make install \
    && ./configure --build=i686-pc-linux-gnu "CFLAGS=-m32" "CXXFLAGS=-m32" "LDFLAGS=-m32" --prefix /opt/i686 --with-shared --with-termlib \
    && make -j 8 \
    && make install

FROM base/archlinux:latest
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"

RUN groupadd --gid 1000 android \
    && useradd \
        --create-home \
        --system \
        --uid 1000 \
        --gid 1000 \
        android

RUN echo "[multilib]" >> /etc/pacman.conf \
        && echo "Include = /etc/pacman.d/mirrorlist" >> /etc/pacman.conf
RUN pacman -Syu --noconfirm \
    && pacman --sync --noconfirm \
        gcc \
        jdk7-openjdk \
        python2 \
        bc \
        rsync \
        zlib \
        libxml2 \
        unzip \
        zip \
        make \
        m4 \
        lib32-glibc \
        lib32-libstdc++5 \
        lib32-zlib


COPY --from=builder /opt/x86_64/lib/libncurses.so.5 /usr/lib/libncurses.so.5
COPY --from=builder /opt/x86_64/lib/libtinfo.so.5 /usr/lib/libtinfo.so.5
COPY --from=builder /opt/i686/lib/libncurses.so.5 /usr/lib32/libncurses.so.5
COPY --from=builder /opt/i686/lib/libtinfo.so.5 /usr/lib32/libtinfo.so.5
RUN ln -s /usr/sbin/python2 /usr/bin/python
RUN ln -s /usr/sbin/make /usr/sbin/make-3.81

USER android
ENV HOME /home/android
WORKDIR $HOME/aosp

ENTRYPOINT [ "bash" ]
