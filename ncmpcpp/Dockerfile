# ncmpcpp is a fully featured MPD client
# which runs in a terminal (using ncurses)
#
# docker run --rm -it \
#       -v /etc/localtime:/etc/localtime:ro \
#       --link mpd:mpd \
#       --name ncmpcpp \
#       martiert/ncmpcpp
#
FROM debian:jessie
LABEL maintainer "Martin Ertsås <martiert@gmail.com>"

RUN useradd --create-home --system --user-group user

RUN apt-get update && apt-get install -y \
        ncmpcpp \
        --no-install-recommends \
        && rm -rf /var/lib/apt/lists/*

USER user
ENTRYPOINT [ "ncmpcpp" ]
CMD [ "--host", "mpd" ]
