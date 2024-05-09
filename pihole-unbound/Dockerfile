FROM pihole/pihole:2024.05.0

RUN echo "deb http://deb.debian.org/debian trixie main" >> /etc/apt/sources.list

RUN apt-get update && apt-get upgrade -y && apt-get -t trixie install -y unbound -V


COPY lighttpd-external.conf /etc/lighttpd/external.conf
COPY unbound-pihole.conf /etc/unbound/unbound.conf.d/pi-hole.conf
COPY 99-edns.conf /etc/dnsmasq.d/99-edns.conf

RUN mkdir -p /etc/services.d/unbound

COPY unbound-run /etc/services.d/unbound/run

RUN chmod 774 /etc/services.d/unbound/run

ENTRYPOINT ./s6-init