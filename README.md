a-look-at-livewire
==================

I recently built an <a href="https://github.com/kylophone/AXIA-LUFS">AoIP EBU R128 meter</a> and along the way dug pretty deep into <a href = "http://en.wikipedia.org/wiki/Livewire_(networking)">Axia Livewire</a>. Here's a look at the protocol. 

###What is Axia Livewire?
Axia Livewire is an IP networked audio protocol used in broadcast. Among other things, it allows for 32767 channels of raw multicast PCM audio distributed over a standard IP network. Axia <a href="http://axiaaudio.com/xnodes">nodes</a> are connected to the network and provide audio I/O.
###What does a Livewire audio stream consist of?
Raw PCM (big endian, 24-bit signed, 48000 kHz, stereo interleaved) delivered in the form of multicast IP/UDP/RTP packets.
###Receiving a stream
To recieve an Axia Livewire stream, your client needs to join the multicast group associated with the Livewire channel you are interested in. The protocol allows for 32767 channels in the multicast range of 239.192.xxx.xxx. Add the channel number to the base ip (239.192.0.0) to find the multicast address. For example, channel number 27 would have a multicast address of 239.192.0.27 while channel number 1212 would be 239.192.4.188.

`livewireStreamMulticastAddr = 0xEFC00000 + ChannelNumber;`
###A look at the packet
coming soon...
