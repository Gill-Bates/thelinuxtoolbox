# Reverse Proxy
A reverse proxy is the state-of-the-art implementation when offering web services on the Internet. On the standard ports `80` or `443`, it is not the actual web server that listens, but the reverse proxy. The reverse proxy also terminates the TLS connection. Internally the reverse proxy routes the request to the actual web server (ngnix, Apache, etc).

## Advantages:

* the client never communicates directly with the web service
* TLS certificates only need to be maintained in one place
* The cache is used to deliver constantly repeated requests through the proxy and saves the resources behind it.

# Tutorial

#### 1. Install the required tools
`sudo apt install squid3`

#### 2. Create a new Config file
`mv /etc/squid/squid.conf /etc/squid/squid.conf.original`
`nano /etc/squid/squid.conf`

Copy & Paste
`
# General Configuration
cache_mem 128 MB

# SSL configuration
#

# Ensure you enter each configuration directive on a single line
acl is_ssl port 443
https_port 443 cert=/etc/pki/tls/certs/lb.crt key=/etc/pki/tls/certs/lb.key accel vhost name=proxy_ssl
cache_peer proxya.example.com parent 443 0 no-query originserver round-robin ssl name=proxya.example.com sslcafile=/etc/pki/tls/certs/squid-ca.crt
cache_peer proxyb.example.com parent 443 0 no-query originserver round-robin ssl name=proxyb.example.com sslcafile=/etc/pki/tls/certs/squid-ca.crt
cache_peer_access proxya.example.com allow is_ssl
cache_peer_access proxya.example.com deny !is_ssl
cache_peer_access proxyb.example.com allow is_ssl
cache_peer_access proxyb.example.com deny !is_ssl
http_access allow is_ssl

#
# Non-SSL configuration
#

# Listening Port
http_port 80 # Listening Port
# Cache in RAM
cache_mem 128 MB # Cache in RAM
# Cache on Disk
cache_dir ufs /var/cache/squid/ 100 16 256 # Cache File Location
# Location Logfiles
cache_access_log /var/log/squid/access.log, cache_log /var/log/squid/cache.log, cache_store_log /var/log/squid/store.log
# Let Logrotate handle the logs
logfile_rotate 0
# Network
client_netmask 255.255.255.255

# Single Host
httpd_accel_single_host on
httpd_accel_host 127.0.0.1
httpd_accel_uses_host_header on


acl nonssl port 80
httpd_accel_single_host on
http_port 80 accel name=proxy_nonssl defaultsite=dhcp16.example.com
cache_peer 127.0.0.1 parent 80 0 no-query name=proxy_nonssl originserver
cache_peer_access proxy_nonssl allow nonssl
cache_peer_access proxy_nonssl deny !nonssl
http_access allow nonssl
sslpassword_program /bin/password.out
forwarded_for on
`
#### 3. Change the default Ports
`nano /etc/apache2/ports.conf`
