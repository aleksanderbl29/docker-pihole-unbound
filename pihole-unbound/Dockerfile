FROM pihole/pihole:2025.02.1
RUN apk add --no-cache unbound

COPY unbound-pihole.conf /etc/unbound/unbound.conf.d/pi-hole.conf
COPY 99-edns.conf /etc/dnsmasq.d/99-edns.conf

RUN exec unbound -c /etc/unbound/unbound.conf.d/pi-hole.conf