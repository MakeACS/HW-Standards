---
icon: lucide/messages-square
---

# 5. Bus Communication

This page details the communication structure used for Communicative Components to interact with the Core. For more information on Communicative Components, see [GPIO](./4-gpio.md).

!!! info
    At this time, only the Half-Duplex communication standard is implemented. Full-Duplex standards will be developed when a valid use-case for them arises.

## Starting Communications

When a Communicative Component boots, it should be in an idle mode, listening on the RS485 interface. The Core will enumerate the OneWire devices on the bus, and look for any conflicts in the GPIO usage. If none are found, it will then move on to assigning addresses to the RS485 devices based on their

## Internal Expansion Communication Interface

In addition to bus communications, the