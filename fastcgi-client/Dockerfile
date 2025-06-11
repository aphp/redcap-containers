FROM alpine:3

LABEL maintainer=APHP

RUN apk update &&\
    apk upgrade &&\
    apk add --no-cache \
        fcgi

RUN adduser -S cgi

USER cgi

ENTRYPOINT ["cgi-fcgi"]
