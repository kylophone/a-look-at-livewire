a-look-at-livewire
==================

I recently built an <a href="https://github.com/kylophone/AXIA-LUFS">AoIP EBU R128 meter</a> and along the way dug pretty deep into <a href = "http://en.wikipedia.org/wiki/Livewire_(networking)">Axia Livewire</a>. Here's a look at the protocol. 

###What is Axia Livewire?
Axia Livewire is an IP networked audio protocol used in broadcast. Among other things, it allows for 32767 channels of raw multicast PCM audio distributed over a standard IP network. Axia <a href="http://axiaaudio.com/xnodes">nodes</a> are connected to the network and provide audio I/O.
###What does a Livewire audio stream consist of?
Raw PCM (big endian, 24-bit signed, 48000 kHz, stereo interleaved) delivered in the form of multicast IP/UDP/RTP packets.
###Receiving a stream
To recieve an Axia Livewire stream, your client needs to join the multicast group associated with the Livewire channel. Add the Axia channel number to the base ip (239.192.0.0) to find its multicast address. Axia channel 27 has a multicast address of 239.192.0.27 and  Axia channel 1212 is 239.192.4.188. See: <a href = "https://gist.github.com/kylophone/a10e2c88ced3bf5e7674">this gist</a>.

`livewireStreamMulticastAddr = 0xEFC00000 + channelNumber;`
###A look at the packet

There are three different types of Livewire streams:

* Standard
* Livestream
* 5.1 Surround

####Standard Livewire Packet

| Function          | Bytes | Notes                                                                                                                   |
|-------------------|-------|-------------------------------------------------------------------------------------------------------------------------|
| Interpacket Delay | 12    | This is not actually transmitted, but is an Ethernet requirement and must be taken into account for bandwidth purposes. |
| Ethernet Header   | 26    | Includes the VLAN/priority fields                                                                                       |
| IP Header         | 20    |                                                                                                                         |
| UDP Header        | 8     |                                                                                                                         |
| RTP Header        | 12    |                                                                                                                         |
| Audio             | 1440  | 240 Samples at 48 kHz, 24 bits, stereo                                                                                  |
| Audio (varient)   | 720   | 120 Samples at 48 kHz, 24 bits, stereo                                                                                  |

Total bytes per packet: 1440, with core delay = 5ms (respective values of 720 bytes and 2.5ms using the variant format)

####Livestream Livewire Packet

| Function          | Bytes | Notes                                                                                                                   |
|-------------------|-------|-------------------------------------------------------------------------------------------------------------------------|
| Interpacket Delay | 12    | This is not actually transmitted, but is an Ethernet requirement and must be taken into account for bandwidth purposes. |
| Ethernet Header   | 26    | Includes the VLAN/priority fields                                                                                       |
| IP Header         | 20    |                                                                                                                         |
| UDP Header        | 8     |                                                                                                                         |
| RTP Header        | 12    |                                                                                                                         |
| Audio             | 72    | 12 Samples at 48 kHz, 24 bits, stereo                                                                                   |

Total bytes per packet = 72, with core delay 2.5ms

####Surround Livewire Packet

| Function          | Bytes | Notes                                                                                                                   |
|-------------------|-------|-------------------------------------------------------------------------------------------------------------------------|
| Interpacket Delay | 12    | This is not actually transmitted, but is an Ethernet requirement and must be taken into account for bandwidth purposes. |
| Ethernet Header   | 26    | Includes the VLAN/priority fields                                                                                       |
| IP Header         | 20    |                                                                                                                         |
| UDP Header        | 8     |                                                                                                                         |
| RTP Header        | 12    |                                                                                                                         |
| Audio             | 1440  | 60 Samples at 48 kHz, 24 bits, stereo + 5.1 (eight channels                                                             |

Total bytes per packet = 1440, with core delay 1.25ms

Surround channels are in the following order: front left, front right, center, low-frequency enhancement (LFE), back left, back right, stereo left, and stereo right.


Packet information taken from the book "Audio Over IP: Building Pro AoIP Systems with Livewire" by Steve Church & Skip Pizzi (2010).