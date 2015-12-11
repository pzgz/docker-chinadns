chinadns + dnsmasq
==================

## About

- chinadns: Protect yourself against DNS poisoning in China.
- dnsmasq: A free software DNS forwarder and DHCP server for small networks.
- Forked from: https://github.com/vimagick/dockerfiles/tree/master/chinadns

## Docker Compose

    chinadns:
      image: vimagick/chinadns
      ports:
        - "53:53/udp"
        - "53:53/tcp"
      restart: always

## Run

**!! Make sure the prepare the host directories which will be mapped into the container**

    docker-compose up -d --name chinadns -p 53:53 -v /volume3/docker/chinadns/dnsmasq.d:/etc/dnsmasq.d

## DNSmasq configurations

A host directory will be mapped to `/etc/dnsmasq.d` in container, so configuration files in this directory will be loaded by dnsmasq.

For me, I will load the files grabbed from [dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)

```
wget https://raw.githubusercontent.com/felixonmars/dnsmasq-china-list/master/google.china.conf
wget https://raw.githubusercontent.com/felixonmars/dnsmasq-china-list/master/accelerated-domains.china.conf
wget https://raw.githubusercontent.com/felixonmars/dnsmasq-china-list/master/bogus-nxdomain.china.conf
```

Same, any extra configurations can be added to this directory.

## Test

    # UDP
    dig @127.0.0.1 www.google.com

    # TCP
    dig @127.0.0.1 www.youtube.com +tcp
