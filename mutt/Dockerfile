# Run Mutt from a container

# docker run -it \
#	-v /etc/localtime:/etc/localtime:ro \
#	-v $HOME/.gnupg:/home/user/.gnupg \ # so you can encrypt ;)
#	--name mutt \
#	jess/mutt
#
FROM alpine:edge
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"

RUN addgroup -g 1000 user \
        && adduser -D -h /home/user -G user -u 1000 user

RUN apk --no-cache add \
        ca-certificates \
        elinks \
        git \
        gnupg1 \
        lynx \
        mutt \
        mutt-doc \
        urlview \
        vim

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
