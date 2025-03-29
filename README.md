# Docker Compose for DNS-Crypt and Pi-Hole 

## Overview
The docker compose file in this repo sets useful default options, such as the dnscrypt server list, and wires pi-hole to use the dnscrypt proxy as its only upstream resolver.

dnsrypt-proxy is a DNS proxy implementation from [dnscrypt-proxy](https://github.com/DNSCrypt/dnscrypt-proxy). It can translate normal unencrypted DNS queries into DNS over TLS queries, DNSCrypt queries, or DNS over HTTP. It also supports DNSSEC.

Using an encrypted DNS protol prevents your ISP from eavesdropping on the sites you visit. Also, since dnscrypt validates the remote endpoint certificate, it helps guard against DNS spoofing. 

[Pi-Hole](https://pi-hole.net/) provides the main DNS service for your network devices. With the Pi-Hole admin Web UI, you can search the DNS query history and manage blocklistis and the application of blocklists to admin-defined groups of clients. Pi-Hole is also extremely fast, especially when queries are resolved from cache, which also persists between container restarts.

The combination of these two containers provides a robust, secure, and private DNS solution.

## Usage

1. Edit the [docker-compose.yml file](docker-compose.yml) to change the Pi-hole admin password and also to place the bind mounted configuration directories wherever you desire. You should also modify the pihole port mappings to expose the DNS ports, e.g. port 53. You may also want to edit the definition of the 'pihole' network. It may be useful, for example, to define this network externally so that you can create stable firewall rules against it.

2. Edit the [dnscrypt-proxy.toml file](dnscrypt-config/dnscrypt-proxy.toml) to change the upstream server list. This is currently set to mullvad-doh and cloudfare. If the line is commented out then the proxy will chose a public server with the least amount of latency. This is not always the best choice since a server that is network-close isn't also guaranteed to be performant. Also, rather than selecting a filtering dhcrypt server, you may wish to manage filtering with pi-hole blocklists as that is its intended use-case.

3. Then run the following 
   ```docker compose up -d```

4. Access the pi-hole admin console at https://localhost:8443 or whichever port you might have chosen.


