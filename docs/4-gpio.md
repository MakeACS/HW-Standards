---
icon: lucide/chevrons-left-right-ellipsis
---

# 4. GPIO

To allow for more versatile, powerful, and application-specific deployments, the MakeACS connector provisions four open-ended GPIO whose use is assigned based on the detected composition of the deployment (see [Component ID](./3-component-id.md)).

All four GPIO are able to operate as either a Schmitt trigger input or 30mA push-pull output at a 5 volt logic level. The Core can dynamically swap between input and output mode to also achieve a Hi-Z "output" state.

Currently, two forms of GPIO operation are supported:

* Multi-Channel Switches allow for up to 4 independent control channels to be activated by the Core.
* Communicative Components can communicate with the Core via a standard, higher-level communication protocol.

!!! warning
    Communicative Components are mutually exclusive with Multi-Channel Switches.

    General Purpose Components (1 Channel Switches, etc.) can be used with either GPIO-using component type.

## Multi-Channel Switches

Multi-Channel Switches are the primary use of the MakeACS GPIO. These allow for up to 4 independent control channels to be activated by the Core, based on different permission levels of the user. As a practical example, this would allow for a single Core to control an entire electronics workbench; it always turns on the power supply and multimeter when someone attempts to access it, but the soldering iron and hot-air rework gun can each be behind their own separate trainings.

When operating as a Multi-Channel Switch, the GPIO number maps to the access control channel, i.e. a 2 Channel Switch would use GPIO 1 and 2 to determine access, and GPIO 3 and 4 are undefined. The Switch must hardware AND the GPIO signal with the Core's main Access signal, such that the loss of either immediately shuts down the output channel.

!!! warning
    To prevent unexpected behavior, a deployment cannot have more than one Multi-Channel Switch. 

!!! note
    While the standard is designed to allow for multi-channel components whose channels are set via communication instead of GPIO, it is not supported at this time.

## Communicative Components

In addition to generic digital signals, the GPIO can be used for higher-level communication between components and the Core. This enables the sending of more complex data, triggering API calls, or setting up more complex logic for a component to achieve a specific goal. 

For more information on the data formatting and structure used, see [Bus Communication](./5-bus-communication).

### Half-Duplex Mode

Half-Duplex Mode allows for one or multiple Communicative Components to communicate with the Core in a shared bus via half duplex RS485. The pinout is as follows;

* **GPIO 1** : RS485 Non-Inverting Input/Output
* **GPIO 2** : RS485 Inverting Input/Output
* **GPIO 3** : Open-Drain Interrupt. Pulled up at the Core (5mA nominal at 5 volts), Components can drive it low to indicate an urgent message.
* **GPIO 4** : Push-Pull Suspend/Reset. Driven by the Core, when low all Communicative Components are expected to enter a suspended state. On the high-going transition of this signal, Components are expected to resume operation in a reset state. This signal is used to fix hanging/broken Components, and to perform regular restarts to clear memory fragmentation.



### Full-Duplex Mode

Full-Duplex Mode allows for one high-bandwidth component to communicate exclusively with the Core.