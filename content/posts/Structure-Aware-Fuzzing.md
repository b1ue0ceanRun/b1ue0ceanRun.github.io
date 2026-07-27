
---
title: "Structure-Aware Fuzzing"
date: 2025-04-20T19:49:50+02:00
tags:
  - fuzz
---

Why I want to talk about the structure aware fuzzing today is because of a black hat video.
https://i.blackhat.com/USA-19/Wednesday/us-19-Metzman-Going-Beyond-Coverage-Guided-Fuzzing-With-Structured-Fuzzing.pdf

what's the difference between structure aware and grammar fuzzing?

<!--more-->

Recently I am working on MQTT protocol fuzzing. But MQTT protocol has some special features.

https://github.com/luisgar1990/MQTTGRAM



## MQTT Protocol
When I am fuzzing mqtt protocol, many testcases will be simplify rejected by the server for the reason: 
- nni recv error!! Connection shudown
- property_parse: Unknown type 7
- UTF-8 check failed!
- pipe id xxx is gone, pub failed

Let's see the description in the MQTT V5 specification:
```
### 2.1.4 Remaining Length

**Position:** starts at byte 2.

The Remaining Length is a Variable Byte Integer that represents the number of bytes remaining within the current Control Packet, including data in the Variable Header and the Payload. The Remaining Length does not include the bytes used to encode the Remaining Length. The packet size is the total number of bytes in an MQTT Control Packet, this is equal to the length of the Fixed Header plus the Remaining Length.
```

Here's a simplified valid MQTT v5 property sequence:
```
0x0B   // Total properties length (var int)
0x01   // Property ID: Payload Format Indicator
0x00   // Value: 0 (binary data)
0x03   // Property ID: Content Type
0x00 0x05  // UTF-8 String Length (5)
74 65 78 74 2F 70 6C 61 69 6E  // "text/plain"
```

So if we do mutation randomly, like bit flipping leads to an invalid input rejected by the target API in the early stage of parsing!

Grammar:

https://github.com/luisgar1990/MQTTGRAM/blob/master/generator.py
## Using libfuzzer for structure aware fuzzing
https://github.com/google/fuzzing/blob/master/docs/structure-aware-fuzzing.md




## Structure Aware VS LLM Generator

pass



https://github.com/MPFuzz/MPFuzz/blob/0d413ccec8855ad356aa382e1709cf28dc3d2622/executable/samples/mqtt_data.xml




mutate如何实现的



## Some solution

Using LLM: https://arxiv.org/html/2501.19282v1



能猜一下 https://github.com/advisories/GHSA-cwr9-w5qw-fr62 怎么出的吗
https://github.com/nanomq/NanoNNG/blob/c86a8064edd6cfcdf706b68ed7eb175d2a77da47/src/sp/protocol/mqtt/mqtt_parser.c

