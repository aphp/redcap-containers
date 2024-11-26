FROM alpine:3

LABEL maintainer=APHP

RUN apk update &&\
    apk add --no-cache fcgi

RUN useradd cgi

USER cgi

ENTRYPOINT ["cgi-fcgi"]
