# Run Mutt from a container

# docker run -it \
#	-v /etc/localtime:/etc/localtime:ro \
#	-v $HOME/.gnupg:/home/user/.gnupg \ # so you can encrypt ;)
#	-v $HOME/.muttrc:/home/user/.muttrc \
#	--name mutt \
#	martiert/mutt
#
FROM debian:jessie
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"

RUN useradd --create-home --system --user-group user

RUN apt update \
        && apt install --yes \
        ca-certificates \
        elinks \
        git \
        gnupg \
        lynx \
        mutt \
        urlview \
        python \
        python-setuptools \
        vim \
        --no-install-recommends \
        && rm --recursive --force /var/lib/apt/lists/*

RUN git clone https://github.com/dmedvinsky/mutt-ics.git /opt/mutt-ics \
        && cd /opt/mutt-ics \
        && python setup.py build \
        && python setup.py install \
        && cd \
        && rm --force --recursive /opt/mutt-ics

# a browser is necessary!
ENV BROWSER lynx
ENV EDITOR vim

USER user
ENV HOME /home/user
ENV TERM xterm-256color
RUN mkdir -p $HOME/.mutt/cache/headers $HOME/.mutt/cache/bodies \
        && touch $HOME/.mutt/certificates

ENV LANG C.UTF-8

ENTRYPOINT [ "mutt" ]
