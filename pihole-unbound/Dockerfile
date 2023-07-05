ARG PIHOLE_VERSION
FROM pihole/pihole:${PIHOLE_VERSION:-latest}
RUN apt update && apt install -y unbound

COPY lighttpd-external.conf /etc/lighttpd/external.conf
COPY unbound-pihole.conf /etc/unbound/unbound.conf.d/pi-hole.conf
COPY 99-edns.conf /etc/dnsmasq.d/99-edns.conf
RUN mkdir -p /etc/services.d/unbound
COPY unbound-run /etc/services.d/unbound/run

ENTRYPOINT ./s6-init

## This is my preferred configuration
ENV WEBTHEME=default-auto
ENV REV_SERVER=false
ENV PIHOLE_DNS=127.0.0.1#5335;1.1.1.1;9.9.9.9
ENV DNSSEC=true