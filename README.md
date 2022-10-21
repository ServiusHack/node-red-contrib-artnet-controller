[![NPM version](https://img.shields.io/npm/v/@bannsaenger/node-red-contrib-artnet-controller.svg)](https://www.npmjs.com/package/@bannsaenger/node-red-contrib-artnet-controller) [![Downloads](https://img.shields.io/npm/dm/@bannsaenger/node-red-contrib-artnet-controller)](https://www.npmjs.com/package/@bannsaenger/node-red-contrib-artnet-controller)

[![NPM](https://nodei.co/npm/@bannsaenger/node-red-contrib-artnet-controller.png?downloads=true)](https://nodei.co/npm/@bannsaenger/node-red-contrib-artnet-controller/)

# @bannsaenger/node-red-contrib-artnet-controller
Node-RED nodes that controls lights via artnet.
**This is a complete solution for sending and receiving dmx-data via Art-Net without the need of additional hardware**

## Credits
[Art-Net™](https://art-net.org.uk/) is a trademark of Artistic Licence Engineering Ltd.

This work is basend on the [node-red-contrib-artnet](https://github.com/gunnebo-ab/node-red-contrib-artnet) nodes developed by [Gunnebo](http://www.gunnebo.com/)

The Art-Net library is taken from [margau/dmxnet](https://github.com/margau/dmxnet)

## Install

Run the following command in the root directory of your Node-RED install. Usually this is `~/.node-red`
```
npm install @bannsaenger/node-red-contrib-artnet-controller
```
Or even inside of Node-RED with the palette manager.

## Using

### Sending DMX-Data

First you must specify a **Art-Net controller** configuration node. The controller is bound to one or all 
ip addresses found on the system and handles the Art-Net polling and shows up as an Art-Net node in the network.

For sending data there must be a **Art-Net Sender** configuration node. This node holds one universe of dmx-data
and hadles the sending of this universe to the network.

To get dmx-data out to the network you use the **Art-Net Out** node. This is basically connected to
a **Art-Net Sender** node. If you mass am msg object without additional addressing, the dmx values
are sent to the **Art-Net Sender** specified in the **Art-Net Out** node.

If you pass additional addressing information, the **Art-Net Out** node tries to find a sender with the given
address information. If none is found the configured **Art-Net Sender** instance is taken.

```
msg.payload = {
  net: 0,       // int: net in [0, 127]
  subnet: 0,    // int: subnet in [0, 15]
  universe: 1   // int: universe in [0, 15]
};
```
### Receiving DMX-Data

With the **Art-Net In** node you can receive dmx values. Each **Art-Net In** node must be bound to a existing **Art-Net controller** configuration node.
You must specify a net, subnet and universe. Then the data can be received in form of
buckets as decribed in **Payload format** later in this document or as a buffer (buffer (Uint8)) with the values of the whole universe.  

### Payload format

You can either set a single channel like the following example:

```
msg.payload = {
  channel: 1, // int: address in [1, 512]
  value: 255  // int: value in [0, 255]
};
```

Or you can set multiple channels at once:

```
msg.payload = {
  buckets: [
    {channel: 1, value: 255},
    {channel: 4, value: 0},
  ]
};
```

You can also fade to values, either for a single channel or multiple channels. You should specify the 'transition' and also a 'duration' in milliseconds:

```
msg.payload={
    transition: "linear",
    duration: 5000,
    buckets: [
      {channel: 1, value: 255},
      {channel: 4, value: 0},
    ]
}
```

In order to perform arc transition (movement by arc) you shold specify more details:

```
msg.payload = {
    transition: "arc",
    duration: 2000,
    arc: {
        pan_channel: 1,
        tilt_channel: 3,
        pan_angle: 540,
        tilt_angle: 255
    },
    start: {pan: 0, tilt: 44},
    center: {pan: 127.5, tilt: 63.75},
    end: {pan: 85, tilt: 44}
};
```
where

- `arc` - channels that should be involved in arc transition (pan and tilt channels)
- `start` - initial channel's values (start point). By default: current channel's values.
- `center` - "center point" values
- `end` - terminal channel's values (end point)

In example above moving head will move by arc starting from {pan: 0, tilt: 44} to {pan: 85, tilt: 44}. Center point ({pan: 127.5, tilt: 63.75}) defines nominal circle center.

## Changelog
<!--
    Placeholder for the next version (at the beginning of the line):
    ### **WORK IN PROGRESS**
-->
### **WORK IN PROGRESS**
* (Bannsaenger) initial release

## License
MIT License

Copyright (c) 2022 Bannsaenger <bannsaenger@gmx.de>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.