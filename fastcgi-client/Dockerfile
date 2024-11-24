FROM alpine:3.18

LABEL maintainer=APHP

RUN apk update &&\
    apk add fcgi

ENTRYPOINT ["cgi-fcgi"]
