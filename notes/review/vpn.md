# IP security protocols (IPsec)

provides security at the network layer

## Virtual Private Networks(VPNs)

can deploy a physical network infrastructure

or create VPNs over the existing public Internet

before data reach public internet, it is encrytpted

the payload of the IPsec datagram includes IPsec header, when it reaches the
destination, the OS in the laptop decrypts the payload and passes the
unencrypted palyload to the upper-layer protocol(like TCP UDP)

> when a source IPsec entity sends secure datagrams to a destination entity, it
> does so with either the AH protocol or the ESP protocol

## Authentication Header (AH) protocol

> provides source authentication and data integrity but does not provide confidentiality

## Encapsulation Security Payload (ESP)

> provides source authentication, data integrity and confidentiality

ESP is more widely used than AH

## Security Association

logical connection between two IPsec entities

if both entities want to send secure datagrams to each other, then two SAs need
to be established, one in each direction

### state information

-   A 32-bit identifier for the SA, called the Security Parameter Index (SPI)
-   The origin interface of the SA (in this case 200.168.1.100) and the destination
    interface of the SA (in this case 193.68.2.23)
-   The type of encryption to be used (for example, 3DES with CBC)
-   The encryption key
-   The type of integrity check (for example, HMAC with MD5) - The authentication key

R1 access this state information whenever it needs to construct IPsec datagram
for forwarding over this SA

An IPsec entity stores the state information for all of its SAs in the Security
Association Database(SAD), which is a data structure in the entity's OS kernel

## IPsec Datagram

two data forms: tunnel mode and transport mode

tunnel mode is more widely used

how original IPv4 datagram is converted to IPsec datagram

-   Appends to the back of the original IPv4 datagram (which includes the original header fields!) an “ESP trailer” field
-   Encrypts the result using the algorithm and key specified by the SA
-   Appends to the front of this encrypted quantity a field called “ESP header”; the resulting package is called the “enchilada”
-   Creates an authentication MAC over the whole enchilada using the algorithm and key specified in the SA
-   Appends the MAC to the back of the enchilada forming the payload
-   Finally, creates a brand new IP header with all the classic IPv4 header fields (together normally 20 bytes long), which it appends before the payload

| new IP header | ESP header | original IP hdr | original IP payload | ESP trailer                    | ESP MAC |
| ------------- | ---------- | --------------- | ------------------- | ------------------------------ | ------- |
| ------------- | SPI + Seq# |                 | ------------------- | padding/pad length/next header |         |

pad-length: indicates to the receiving entity how much padding was inserted(and
thus needs to be removed)

next header: identifies the type (e.g., UDP)

SPI: indicated to the receiving entity the SA to which the datagram belongs;

sequence number: used to defend against replay attacks

ESP MAC: used to verify that enchilada is consistent comes from R1 and has not
been tampered with

### Security Policy Database (SPD\_)

> indicates "what" to do with an arriving datagram

the SPD indicates what types of datagrams (as a function of source IP address,
destination IP address, and protocol type) are to be IPsec processed

## Internet Key Exchange (IKE)

managing SPI
