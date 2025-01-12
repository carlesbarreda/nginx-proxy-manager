FROM jc21/alpine-nginx-full:node
LABEL maintainer="Jamie Curnow <jc@jc21.com>"

ENV S6_LOGGING=0
ENV SUPPRESS_NO_CONFIG_WARNING=1
ENV S6_FIX_ATTRS_HIDDEN=1

RUN echo "fs.file-max = 65535" > /etc/sysctl.conf \
	&& apk add --no-cache python3=~3 certbot=~1 jq=~1 \
	&& python3 -m ensurepip \
	&& pip3 install git+https://github.com/rdrgzlng/certbot-dns-dinahosting.git@master

# Task
RUN cd /usr \
	&& curl -sL https://taskfile.dev/install.sh | sh \
	&& cd /root

COPY rootfs /
RUN rm -f /etc/nginx/conf.d/production.conf

# s6 overlay
RUN curl -L -o /tmp/s6-overlay-amd64.tar.gz "https://github.com/just-containers/s6-overlay/releases/download/v1.22.1.0/s6-overlay-amd64.tar.gz" \
	&& tar -xzf /tmp/s6-overlay-amd64.tar.gz -C /

EXPOSE 80
EXPOSE 81
EXPOSE 443

ENTRYPOINT [ "/init" ]

HEALTHCHECK --interval=5s --timeout=3s CMD /bin/check-health