# Computer Networks
---
### Missed 2 Lectures
---
##### September 2nd, 2022
---
### Chapter 3 (cont.)

Last time, DNS, P2P, Video and CDN

#### UDP
UDP sockets use a 2-tuple, destination IP/port. Simple with low overhead, can be made reliable but up to the application. Destination port on the receiving side directs the connection.

Datagram starts with 64 bit header
- 16 bits, source & destination ports
- 16 bits, length & checksum
- Length includes all the header's info

Checksum is all 16-bit words (with carry), takes one complemet. Easy to check at other end. Just add all words AND the checksum, should be all 1's.

UDP checksums should always be enabled (normally are). Other layers may add checksums as well. It should provide a valid end-to-end transfer. When the checksum is bad, we either throw it out or sending a warning. Only one bit can be bad and it still be acceptable. 

##### UDP Advantages

Finer level of control over what data is sent and when. No connection establishment, no handshake, just send. No connection state. Smaller packet overhead, only 8 bytes, TCP uses 20.

Need to be careful not to exceed the transport layer MTU (maximum transport unit). For OP this is normally 1500 bytes but could be very small.

...

On Linux _netstat -i_ will give this value. 

...

#### TCP

TCP is reliable and provides congestion control. 

The socket setup requires a 4-tuple
- source IP/port
- destination IP/port

Reliable transmission, one packet version
- receives data from application
- generates header and checksum (for detecting data errors at the other end)
- sends packet
- while !ACK send packet

Reliable reception, one packet version
- receive packet
- while corrupt, send NAK
- else send ACK
- extract data and deliver to application

