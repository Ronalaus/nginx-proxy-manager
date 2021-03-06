FROM jc21/nginx-full:node
LABEL maintainer="Jamie Curnow <jc@jc21.com>"

ENV S6_LOGGING=0 \
	SUPPRESS_NO_CONFIG_WARNING=1 \
	S6_FIX_ATTRS_HIDDEN=1

RUN echo "fs.file-max = 65535" > /etc/sysctl.conf \
	&& apt-get update \
	&& apt-get install -y certbot jq python3-pip \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

# Task
RUN cd /usr \
	&& curl -sL https://taskfile.dev/install.sh | sh \
	&& cd /root

COPY rootfs /
RUN rm -f /etc/nginx/conf.d/production.conf

# s6 overlay
RUN curl -L -o /tmp/s6-overlay-amd64.tar.gz "https://github.com/just-containers/s6-overlay/releases/download/v1.22.1.0/s6-overlay-amd64.tar.gz" \
	&& tar -xzf /tmp/s6-overlay-amd64.tar.gz -C /

EXPOSE 80 81 443
ENTRYPOINT [ "/init" ]
HEALTHCHECK --interval=5s --timeout=3s CMD /bin/check-health
