example based on RS232 physical transformer.

suppose:

+ use rs232 to transfer five kinds of sensor data
+ each kind of data has a specific processer
+ use xor to check data


basic two methods for rs232 supported:

+ rs.send(bytearray)
+ rs.read()

Let's start!

# Content

+ RS232 Introduction
+ Pack A Sensor Data
+ Unpack A Package
+ Send A Kind Of Data To Specific Processer

# RS232 Brief Introduction

RS232 is a simple protocol used in low speed, small data transform scene. It widely used in transfrom data between embedded system and computer system. A network administrator always use the RS232 to check the setting of the network router old days.

feature:

- support 9600 - 115200 bits/s
- support Even/Odd check
- support 7/8 bit data each package
- support two-wired simple transforming

our example base on this protocol use 115200, 8 bit, No-check.

# Pack A Sensor Data

suppose we get a camera, after we collect image data, we have to transform to a embedded system use program-1 to process the camera data.

we have to let the embedded system to know:

+ what kind of sensor data
+ who to process the data

but rs232 just support max 8 bit each package, so we can just send the camera data as a single rs232 package. we need our own pakcage, we have to define our package format, our camera data finally is sliced into several rs232 packages.


| sync | len | address | kind | playload | check |
|:----:|:---:|:-------:|:----:|:--------:|:-----:|
|2 bytes|4 bytes|1 byte|2 bytes| -- |1 byte|

pckage format:

1. **sync**: a package start with `0xFF 0xFE`, use to detect a package start.
2. **len**: record playload length.
3. **address**: which application to process the data.
4. **kind**: what kind of sensor data.
5. **playload**: camera image data.
6. **check**: xor check for the whole package.

a simple pack program without test:


``` python
# -*- coding: utf-8 -*-

import binascii
import StringIO

def pack(address, kind, data):
    """pack the sensor data into package"""

    buffer = StringIO.StringIO()

    # for simple
    # len = len(data)
    # buffer.write("{sync:04x}{len:08x}{address:02x}{kind:04x}{playload:x}".format(
    #                   sync=0xfffe,
    #                   len=len,
    #                   address=address,
    #                   kind=kind,
    #                   playload=data))

    # write sync
    buffer.write("{:04x}".format(0xfffe))
    # write len
    buffer.write("{:08x}".format(len(data)))
    # write address
    buffer.write("{:02x}".format(address))
    # write kind
    buffer.write("{:04x}".format(kind))
    # write data
    buffer.write("{:x}".format(data))

    check = 0x00
    # or you can use reduce
    # reduce(lambda x, y: x ^ y, data)
    for elem in xrange(data):
        check ^= elem

    # write check
    buffer.write("{:02x}".format(check))

    return binascii.unhexlify(buffer.getvalue())

```

# Unpack A Package

when you receive a package from the other side, you need to unpack the package to get the sensor data.

if the package is small you can just use offset to get the different part out. sometime it's a good way to unpack the data use a state  machine.

let's start with the simple way:

``` python
# -*- coding: utf-8 -*-

"""
use plain code to unpack the data
"""

import binascii

# only a complete package here
package = '0xFF0xFE0x12...'

# read every part out from the package
rules = [
    (2, 4), (6, 1), (7, 2), (9, 0)
]

buffer = binascii.hexlify(package)

buffer = buffer[2:-2]

parts = []

for offset, len in rules:
    if len == 0:
        parts.append(buffer[offset:])
    else:
        parts.append(buffer[offset:offset+len])

len, address, kind, data = parts

```