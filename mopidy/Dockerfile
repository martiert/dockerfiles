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

RUN apt-get update && apt-get install --yes curl
RUN curl https://apt.mopidy.com/mopidy.gpg | apt-key add -
RUN curl --output /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/jessie.list
RUN apt-get update && apt-get install --yes \
        ca-certificates \
        mopidy \
        mopidy-spotify \
        mopidy-soundcloud \
        mopidy-spotify-tunigo \
        mopidy-youtube

# Make a user
ENV HOME /home/mopidy
RUN mkdir $HOME $HOME/.config \
        && usermod --append --groups audio mopidy \
        && chown --recursive mopidy:root $HOME

WORKDIR $HOME
USER mopidy

ENTRYPOINT [ "mopidy", "--config", "/home/mopidy/.config/mopidy/mopidy.conf" ]
