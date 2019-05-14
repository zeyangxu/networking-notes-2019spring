# TCP Connection-Oriented-Transport

full-duplex service: bi-direction data flow at the same time

end-to-end: single sender and single receiver

## establish

client process: process that is initiating the connection

```python
clientSocket.connect((serverName, serverPort))
```

### three-way handshake

client -- sync(no payload) --> server
client <- sync ack(no payload) -- server
client -- ack(with payload) --> server

## Segment Structure

-   32-bit sequence & acknowledgement number: ensure reliable data
    transfer
-   16-bit receive window: used for flow control
-   4-bit header length field: specifies lenght of TCP header in 32-bit words
-   options
-   flag-fields: 6 bits
    ACK: indicates successful receive
    RST, SYN, FIN: connection setup and teardown
    PSH: indicate the receiver should pass data to upper layer
    URG: indicate there is data which sending-side upper layer mark as urgent

### sequence number

byte-stream number of the first byte in the segment sent

e.g.

file: 500,000 bytes MSS: 1,000 bytes

500 segments: #0, #1000, #2000, #3000...

### acknowledge numbers

is the sequence number of the next byte Host A is expecting from Host B

## Fast Retransmit

before timeout, if out of order packet is detected, send a duplicate ACK to tell
sender to resend everything after the ACK

## Flow control

receiver's buffer could be overflow, if data reading is slower than the data sending

### receive window

maintained by sender

A send a large file to B: A => B

In proceess B

LastByteRcvd - LastByteRead <= RcvBuffer

rwnd = RcvBuffer - [LastByteRcvd - LastByteRead]

> rwnd is the spare space in buffer

In process A

LastByteSent - LastByteAcked <= rwnd

### problem

when process B is full, it rwnd is zero, A would stop sending data, but even after B clear out the buffer, A would not be acknoledged to continue

so TCP specification requires sender keep sending one data byte when receiver's buffer is full

## close of the connection

client -FIN-> Server
client <-ACK- Server
client <-FIN- Server (close)
client -Ack-> Server
client Timed wait -> client (close)
