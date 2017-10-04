# Run mopidy in a container
#
# docker run -it --rm \
#	--volume /etc/localtime:/etc/localtime:ro \
#	--volume $HOME/.config/mopidy/mopidy.conf:/etc/mopidy/mopidy.conf \ # mount config files
#	--device /dev/snd \
#	--publish 6600:6600 \
#	--name mopidy \
#	martiert/mopidy
#
FROM debian:jessie
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"
ARG AUDIO_GID

RUN apt-get update && apt-get install --yes curl
RUN curl https://apt.mopidy.com/mopidy.gpg | apt-key add -
RUN curl --output /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/jessie.list
RUN apt-get update && apt-get install --yes \
        ca-certificates \
        mopidy \
        mopidy-spotify \
        mopidy-soundcloud \
        mopidy-spotify-tunigo \
        mopidy-youtube \
        alsa-utils

# Make a user
ENV HOME /home/mopidy
RUN mkdir $HOME $HOME/.config \
        && chown -R mopidy:root $HOME
RUN usermod -a -G audio mopidy \
        && groupmod -g $AUDIO_GID audio

WORKDIR $HOME
USER mopidy

ENTRYPOINT [ "mopidy" ]
