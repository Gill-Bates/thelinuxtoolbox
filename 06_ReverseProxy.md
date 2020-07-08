# Reverse Proxy
A reverse proxy is the state-of-the-art implementation when offering web services on the Internet. On the standard ports `80` or `443`, it is not the actual web server that listens, but the reverse proxy. The reverse proxy also terminates the TLS connection. Internally the reverse proxy routes the request to the actual web server (ngnix, Apache, etc).

## Advantages:

* the client never communicates directly with the web service
* TLS certificates only need to be maintained in one place
* The cache is used to deliver constantly repeated requests through the proxy and saves the resources behind it.
