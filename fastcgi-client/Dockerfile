FROM alpine:3

LABEL maintainer=APHP

RUN apk update &&\
    apk add --no-cache fcgi

ENTRYPOINT ["cgi-fcgi"]
