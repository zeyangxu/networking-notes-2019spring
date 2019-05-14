# DNS

## DNS Records and Messages

resource record: (Name, Value, Type, TTL)

Type of records

-   A: hostname-to-IP mapping, Name is hostname, Value is IP

(relay1.bar.foo.com, 145.37.93.126, A)

-   NS: Name is domain, Value next DNS server that knows IP address, used to
    forward

(foo.com, dns.foo.com, NS)

-   CNAME: value is a canonical hostname for the alias hostname

(foo.com, relay1.bar.foo.com, CNAME)

-   MX: the value is the canonical name of a mail server that has an alias
    hostname

(foo.com, mail.bar.foo.com, MX)
