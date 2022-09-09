# Computer Networks
##### September 7th, 2022
---

### Chapter 3, More TCP

#### Header Fields
- Header Length, 4 bits
	- Specifices how many 32 bit words in header
- Unused 6 bits
- Flags, 6 bits

#### Flags
- URG, the sender-upper (application) layer-thinks this is "urgent."
- ACK, ACK field is valid.
- PSH, receiver pass data up immediately, do not buffer.
- RST, SYN, FIN are for connection setup and teardown
- RST basically for "not accepting connections."
