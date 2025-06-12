# Build Container Image
FROM alpine as cmatrixbuilder

LABEL org.opencontainers.image.authors="Sebastian Nowak" \
      org.opencontainers.image.description="Container image for https://github.com/abishekvashok/cmatrixe"

WORKDIR cmatrix

RUN apk update --no-cache && \
 apk add git autoconf automake alpine-sdk ncurses-dev ncurses-static && \
 git clone https://github.com/spurin/cmatrix.git . && \
 mkdir -p /usr/lib/kbd/consolefonts /usr/share/consolefonts && \
 autoreconf -i && \
 ./configure LDFLAGS="-static" &&\
  make


# cmatrix container image
FROM alpine

LABEL org.opencontainers.image.authors="Sebastian Nowak" \
      org.opencontainers.image.description="Container image for https://github.com/abishekvashok/cmatrixe"

RUN apk update --no-cache && \ 
    apk add ncurses-terminfo-base && \
    adduser -g "Sebastian Nowak" -s /usr/sbin/nologin -D -H sebastian
COPY --from=cmatrixbuilder /cmatrix/cmatrix /cmatrix

USER sebastian
ENTRYPOINT ["./cmatrix"]
CMD ["-b"]
