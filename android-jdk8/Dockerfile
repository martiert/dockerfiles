FROM base/archlinux:latest as builder

RUN pacman -Syu --noconfirm \
    && pacman --sync --noconfirm \
        gnupg \
        wget \
        gcc \
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
        &&  mkdir /tmp/ncurses \
        && cd /tmp/ncurses \
        && wget https://aur.archlinux.org/cgit/aur.git/snapshot/ncurses5-compat-libs.tar.gz \
        && tar xf ncurses5-compat-libs.tar.gz \
        && cd ncurses5-compat-libs \
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
        git \
        openssl-1.0 \
        python \
        gcc \
        jdk8-openjdk

COPY --from=builder /tmp/ncurses/ncurses5-compat-libs/ncurses5-compat-libs*.pkg.tar.xz /tmp/ncurses5-compat-libs.pkg.tar.xz
RUN  cd /tmp \
        && pacman --upgrade --noconfirm ncurses5-compat-libs.pkg.tar.xz \
        && rm ncurses5-compat-libs.pkg.tar.xz

USER android
ENV HOME /home/android
RUN mkdir $HOME/.gradle
WORKDIR $HOME

ENTRYPOINT [ "bash" ]
