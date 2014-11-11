a-look-at-livewire
==================

I recently built an <a href="https://github.com/kylophone/AXIA-LUFS">AoIP EBU R128 meter</a> and along the way dug pretty deep into <a href = "http://en.wikipedia.org/wiki/Livewire_(networking)">Axia Livewire</a>. Here's a look at the protocol. 

###What is Axia Livewire?
Axia Livewire is an IP networked audio protocol used in broadcast. Among other things, it allows for 32767 channels of raw multicast PCM audio distributed over a standard IP network. Axia <a href="http://axiaaudio.com/xnodes">nodes</a> are connected to the network and provide audio I/O.
###What does a Livewire audio stream consist of?
Raw PCM (big endian, 24-bit signed, 48000 kHz, stereo interleaved) delivered in the form of multicast IP/UDP/RTP packets.
###Receiving a stream
To recieve an Axia Livewire stream, your client needs to join the multicast group associated with the Livewire channel. The protocol allows for 32767 channels in the multicast range of 239.192.xxx.xxx. Add the Axia channel number to the base ip (239.192.0.0) to find its multicast address. Axia channel 27 has a multicast address of 239.192.0.27 and  Axia channel 1212 is 239.192.4.188. See: <a href = "https://gist.github.com/kylophone/a10e2c88ced3bf5e7674">this gist</a>.

`livewireStreamMulticastAddr = 0xEFC00000 + channelNumber;`
###A look at the packet
coming soon...
