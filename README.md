# udp-ue4
Convenience ActorComponent UDP wrapper for Unreal Engine 4.

[![GitHub release](https://img.shields.io/github/release/getnamo/udp-ue4.svg)](https://github.com/getnamo/udp-ue4/releases)
[![Github All Releases](https://img.shields.io/github/downloads/getnamo/udp-ue4/total.svg)](https://github.com/getnamo/udp-ue4/releases)

This may not be the most sensible wrapper for your use case, but is meant to co-exist with https://github.com/getnamo/socketio-client-ue4 with similar workflow.

Wraps built-in ue4 udp functionality as an actor component with both sending and receiving capabilities. Confirmed working with node.js [dgram](https://nodejs.org/api/dgram.html) (see [example echo server gist](https://gist.github.com/getnamo/8117fdc64209af086ce0337310c52a51) for testing).

## Quick Install & Setup

 1. [Download Latest Release](https://github.com/getnamo/udp-ue4/releases)
 2. Create new or choose project.
 3. Browse to your project folder (typically found at Documents/Unreal Project/{Your Project Root})
 4. Copy *Plugins* folder into your Project root.
 5. Plugin should be now ready to use.
 
## How to use - Basics
 
Select an actor of choice. Add UDP component to that actor.
 
![add component](https://i.imgur.com/EnCiU4K.png)
 
Select the newly created component and modify any default settings
 
![defaults](https://i.imgur.com/wyYN2pU.png)

By default the udp actor component will auto open both send and receive sockets on begin play. If you're only interested in sending, untick should auto open receive and modify your send ip/port to match your desired settings. Conversely you can untick auto open send if you're not interested in sending.
 
Also if you want to connect/listen on your own time untick both and connect manually via e.g. key event
 
![manual open receive](https://i.imgur.com/HkSvGCb.png)
 
Receive Ip of 0.0.0.0 will listen to all connections on specified port.
 
### Sending
 
Once your sending socket is opened (more accurately prepared sending socket, since you don't get a callback in UDP like in TCP), use emit to send some data, utf8 conversion provided by socket.io plugin. NB: if you forgot to open your socket, emit will auto-open on default settings and emit.
 
![emit](https://i.imgur.com/OzG0caw.png)
 
returns true if the emit processed. NB: udp is unreliable so this is not a return that the data was received on the other end, for a reliable connection consider TCP.
 
### Receiving
 
![events](https://i.imgur.com/1mdlQdI.png)
 
Once you've opened your receive socket you'll receive data on the ```OnReceivedBytes``` event
 
![receive bytes](https://i.imgur.com/1Lq0mDg.png)
 
which you can convert to convenient strings or structures via socket.io (optional and requires your server sends data as JSON strings).

#### Receiving on Bound Send port

When you open a send port it will generate a bound send port which you can use to listen. This should help NAT piercing. To use this feature untick should auto open receive and open your receive socket on the send socket open event with the bound port.

![image](https://user-images.githubusercontent.com/542365/112771022-7c8c1900-8fde-11eb-971e-e81c3d4e55cd.png)

 
### Reliable Stream
 
Each release includes the socket.io client plugin, that plugin is intended to be used for reliable control and then real-time/freshest data component of your network can be piped using this udp plugin. Consider timestamping your data so you can know which packets to drop/ignore.

## Packaging

### C++
Works out of the box.

### Blueprint
If you're using this as a project plugin you will need to convert your blueprint only project to mixed (bp and C++). Follow these instructions to do that: https://allarsblog.com/2015/11/04/converting-bp-project-to-cpp/

![Converting project to C++](https://i.imgur.com/Urwx2TF.png)

e.g. Using the File menu option to convert your project to mixed by adding a C++ file.

## Notes
MIT licensed.

Largely inspired from https://wiki.unrealengine.com/UDP_Socket_Sender_Receiver_From_One_UE4_Instance_To_Another.
