# docker build -t ex00 .
# docker run -it ex00
# esc : "w filename"

FROM alpine

RUN apk update \
	&& apk upgrade \
	&& apk add vim

ENTRYPOINT vim
