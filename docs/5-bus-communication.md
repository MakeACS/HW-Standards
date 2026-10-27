---
icon: lucide/messages-square
---

# 5. Bus Communication

This page details the communication structure used for Communicative Components to interact with the Core. For more information on Communicative Components, see [GPIO](./4-gpio.md).

!!! info
    At this time, only the Half-Duplex communication standard is implemented. Full-Duplex standards will be developed when a valid use-case for them arises.

## Payload Structure

Message payloads use a COBS-Framed Binary Packet, with 0x00 serving as the delimiter at the start and end of a payload. The structure is as follows;

``` 
[0x00][Header][Payload][CRC16][0x00]
```

The Header has the following information;

* Target (1 byte): The device the communication is intended for. 0xFF is reserved for sending to the Core, and 0xFE is reserved for sending to all devices (broadcast). 
* Control Flags (1 byte): This has information related to the payload's purpose and usage.
    * Bit 7: Framing Type. 0 represents data, 1 represents a control/command.
    * Bit 6: ACK Requested. 1 represents yes.
    * Big 5: Retransmit Flag. 1 represents this is not the original transmission of this data.
    * Bit 4-0: Sequence Number. 0-31, increments per message sent by that sender. Rolls over at 31.
* Message Type (1 byte): Represents what kind of information is being sent.


## Starting Communications

When a Communicative Component boots, it should be in an idle mode, listening on the RS485 interface. The Core will enumerate the OneWire devices on the bus, and look for any conflicts in the GPIO usage. If none are found, it will then move on to assigning short addresses to the RS485 devices based on their OneWire addresses. These short addresses are sequential from 0x01 to *0xN*, and are used for addressing the devices during communication. 

Until a component is assigned an address, it should assume it is part of an invalid or non-functional deployment, and not do anything. 

## Internal Expansion Communication Interface

In addition to bus communications, the