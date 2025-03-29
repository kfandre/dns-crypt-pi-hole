# Docker Compose for DNS-Crypt and Pi-Hole 

## Overview
The docker compose file in this repo sets suitable default options -- such as the dnscrypt server list -- and wires pi-hole to use the dnscrypt proxy as its only upstream resolver.

dnsrypt-proxy is a DNS proxy implementation from [dnscrypt-proxy](https://github.com/DNSCrypt/dnscrypt-proxy). It can translate normal unencrypted DNS queries into DNS over TLS queries, DNSCrypt queries, or DNS over HTTP.

Using an encrypted DNS connection prevents your ISP from eavesdropping on the sites you visit. Also, since dnscrypt validates the remote TLS endpoint certificate against a CA database, it helps guard against DNS spoofing. 

## Usage

1. Edit the [docker-compose.yml file](docker-compose.yml) to change the pi-hole admin password and also to place the bind mounted configuration directories wherever you desire. You should also modify the pihole port mappings to expose the DNS ports at whever port you need, e.g. port 53.

2. Edit the [dnscrypt-proxy.toml file](dnscrypt-config/dnscrypt-proxy.toml) to change the servers to whichever servers you would like. This is currently set to mullvad-doh and cloudfare. If the line is commented out then the proxy will chose a server with the least amount of latency. This is not always the best choice since a server that is network-close isn't also guaranteed to be performant. Rather than selecting filtering services, you may wish to add your own blocklists to pi-hole as that is its intended use-case.

3. Then run the following ```docker compose up -d```

4. Access the pi-hole admin console at https://localhost:8443 or whichever port you might have chosen.


