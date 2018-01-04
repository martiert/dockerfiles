# docker run \
#     --detach \
#     --rm \
#     --volume /tmp/.X11-unix:/tmp/.X11-unix \ # X11 access
#     --volume $HOME/.android:/home/android/.android \ # Keep settings
#     --volume $HOME/.AndroidStudio3.0:/home/android/AndroidStudio3.0 \ # Keep even more settings
#     --env DISPLAY=unix$DISPLAY \ # For access to the display
#     --name android-studio \
#     martiert/android-studio
FROM alpine:3.5 as builder
RUN apk add --no-cache curl unzip

RUN mkdir opt \
    && curl --output /opt/android-studio.zip https://dl.google.com/dl/android/studio/ide-zips/3.0.1.0/android-studio-ide-171.4443003-linux.zip \
    && cd /opt \
    && unzip android-studio.zip

FROM debian:jessie
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"

RUN groupadd --gid 1000 user \
        &&  useradd \
            --create-home \
            --system \
            --uid 1000 \
            --gid 1000 \
            android

RUN apt update \
        && apt install --yes \
            lib32z1 \
            git \
            default-jre \
            default-jdk \
            --no-install-recommends \
            && rm --recursive --force /var/lib/apt/lists/*

USER android
ENV HOME /home/android
COPY --from=builder /opt/android-studio $HOME/.local/opt/android-studio

ENTRYPOINT [ "/home/android/.local/opt/android-studio/bin/studio.sh" ]
