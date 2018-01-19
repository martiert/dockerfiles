# Pulseaudio
#
# docker run -d \
#	-v /etc/localtime:/etc/localtime:ro \
#	--device /dev/snd \
#	--name pulseaudio \
#	-v /var/run/dbus:/var/run/dbus \
#	-v /etc/machine-id:/etc/machine-id \
#	martiert/pulseaudio
#
FROM debian:jessie
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"

RUN apt-get update && apt-get install -y \
        alsa-utils \
        libasound2 \
        libasound2-plugins \
        pulseaudio \
        pulseaudio-utils \
        dbus-x11 \
        --no-install-recommends \
        && rm -rf /var/lib/apt/lists/*

ENV HOME /home/pulseaudio
RUN useradd --create-home --home-dir $HOME pulseaudio \
        && usermod -aG audio,pulse,pulse-access pulseaudio \
        && chown -R pulseaudio:pulseaudio $HOME

WORKDIR $HOME
USER pulseaudio

COPY default.pa /etc/pulse/default.pa
COPY client.conf /etc/pulse/client.conf
COPY daemon.conf /etc/pulse/daemon.conf
copy start_pulseaudio /usr/bin/start_pulseaudio

EXPOSE 4713
ENTRYPOINT [ "start_pulseaudio" ]
