# DHCP

Dynamic Host Configuration Protocol

## Procedure

-   DHCP Server <--discover- client

this is a broadcast

send a UDP packet to port 67

destination IP: 255.255.255.255

source IP: 0.0.0.0

-   Server --offer-> client

this is also a broadcast

-   Server <-request- client

after client choose one DHCP server

-   Server --ACK--> client
