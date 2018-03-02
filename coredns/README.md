CoreDNS

This Dockerfile creates a CoreDNS instance that listens to 53/tcp and 53/udp, and forwards all requests to either [Quad9 using DNS over TLS](https://www.quad9.com/faq/#Does_Quad9_support_DNS_over_TLS), or Google's [DNS-over-HTTPS](https://developers.google.com/speed/public-dns/docs/dns-over-https). This allows you to have a little more privacy in your DNS queries, since DNS is usually a plaintext protocol.

```
docker build -t private-coredns .
docker run -d -p 53:53 -p 53:53/udp private-coredns
dig @127.0.0.1 +noall +answer google.com
dig @127.0.0.1 +noall +answer google.com AAAAA
dig @127.0.0.1 +noall +answer microsoft.com
dig @127.0.0.1 +noall +answer microsoft.com AAAA
```

