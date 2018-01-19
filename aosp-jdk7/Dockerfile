FROM base/archlinux:latest as builder

RUN echo "[multilib]" >> /etc/pacman.conf \
        && echo "Include = /etc/pacman.d/mirrorlist" >> /etc/pacman.conf

RUN pacman -Syu --noconfirm \
    && pacman --sync --noconfirm \
        lib32-glibc \
        lib32-ncurses \
        wget \
        gcc-multilib \
        multilib-devel \
        make \
        fakeroot \
        binutils

RUN groupadd --gid 1000 android \
    && useradd \
        --create-home \
        --system \
        --uid 1000 \
        --gid 1000 \
        builder

USER builder
RUN gpg --keyserver pool.sks-keyservers.net --recv-keys 702353E0F7E48EDB \
        && mkdir /tmp/ncurses /tmp/lib32-ncurses \
        && cd /tmp/ncurses \
        && wget https://aur.archlinux.org/cgit/aur.git/snapshot/ncurses5-compat-libs.tar.gz \
        && tar xf ncurses5-compat-libs.tar.gz \
        && cd ncurses5-compat-libs \
        && makepkg \
        && cd /tmp/lib32-ncurses \
        && wget https://aur.archlinux.org/cgit/aur.git/snapshot/lib32-ncurses5-compat-libs.tar.gz \
        && tar xf lib32-ncurses5-compat-libs.tar.gz \
        && cd lib32-ncurses5-compat-libs \
        && makepkg


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


COPY --from=builder /tmp/ncurses/ncurses5-compat-libs/ncurses5-compat-libs*.pkg.tar.xz /tmp/ncurses5-compat-libs.pkg.tar.xz
COPY --from=builder /tmp/lib32-ncurses/lib32-ncurses5-compat-libs/lib32-ncurses5-compat-libs*.pkg.tar.xz /tmp/lib32-ncurses5-compat-libs.pkg.tar.xz
RUN  cd /tmp \
        && pacman --upgrade --noconfirm ncurses5-compat-libs.pkg.tar.xz lib32-ncurses5-compat-libs.pkg.tar.xz \
        && rm ncurses5-compat-libs.pkg.tar.xz lib32-ncurses5-compat-libs.pkg.tar.xz

RUN ln -s /usr/sbin/python2 /usr/bin/python
RUN ln -s /usr/sbin/make /usr/sbin/make-3.81

USER android
ENV HOME /home/android
WORKDIR $HOME/aosp

ENTRYPOINT [ "bash" ]
