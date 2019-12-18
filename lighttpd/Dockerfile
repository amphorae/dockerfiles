FROM alpine:3

ENV LIGHTTPD_VERSION=1.4.54-r0

RUN apk add --update --no-cache lighttpd=$LIGHTTPD_VERSION

EXPOSE 80

VOLUME [ "/etc/lighttpd", "/var/www/" ]

ENTRYPOINT [ "lighttpd", "-D", "-f", "/etc/lighttpd/lighttpd.conf"]