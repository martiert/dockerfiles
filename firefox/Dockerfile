# docker run \
#     --detach \
#     --rm \
#     --privileged \ # For access to drivers for fido u2f
#     --volume /dev:/dev:ro \ # For fido u2f
#     --volume /tmp/.X11-unix:/tmp/.X11-unix \ # X11 access
#     --volume $HOME/.mozilla:/home/user/.mozilla \ # Keep settings
#     --volume $HOME/Downloads:/home/user/Downloads \ # Nice to have for downloads
#     --link pulseaudio:pulseaudio \ # Firefox after 52 uses pulseaudio
#     --env PULSE_SERVER=pulseaudio \
#     --env DISPLAY=unix$DISPLAY \ # For access to the display
#     --name firefox \
#     martiert/firefox
FROM debian:sid
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"

RUN groupadd --gid 1000 user \
        &&  useradd \
            --create-home \
            --system \
            --uid 1000 \
            --gid 1000 \
            --groups audio,video \
            user

RUN apt update \
        && apt install --yes \
            firefox \
            pulseaudio \
            --no-install-recommends \
            && rm --recursive --force /var/lib/apt/lists/*

USER user
ENV HOME /home/user
ENTRYPOINT [ "firefox" ]
