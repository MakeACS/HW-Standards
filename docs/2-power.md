---
icon: lucide/zap
---

# 2. Power

One of the main benefits of MakeACS is the decentralized power. In most deployments, the power required to operate the system can be harvested directly by the Switches to power the Core and other components. 

## Power Rails

The MakeACS bus has two power rails; a nominal 12 volt one and a nominal 5 volt one. Both power rails are rated for up to 2 amps each.

The 12 volt rail is generically the high-power rail. Components may supply any voltage they have available between 9 and 12 volts to this rail. The 5 volt rail has tighter control for logic-level operation, and components cannot provide anything outside of 4.8 to 5.2 volts to this rail. 

The Core is required to provide a regulator that will make available at least 1 amp on the 5 volt rail from the 12 volt rail, for deployment where there are 12 volt supplying devices but no 5 volt supplying devices.

Components may use the 12 volt rail for high-current applications like large lights, displays, motors, etc., while the 5 volt rail is reserved for minimal draw to power logical circuitry. Any component with sustained 5 volt loads in excess of 250mA should implement their own regulator from the 12 volt rail.

## Rail Connections

Rails operates as a decentralized ideal diode OR. All power-supplying components attaching to the rail are required to do so though an ideal diode, such that power supplies can load share or dynamically be replaced as they are superseded or removed from the bus. Reference components for this application are the [MX66100T](https://www.lcsc.com/product-detail/C53121238.html) for low-current (USB or similar) implementations, and the [MX5052S](https://www.lcsc.com/product-detail/C5875696.html) for most other implementations.

## Supply Requirements

Any component that is capable of supplying power is required do to do, and must provide as much as possible (up to the 2 amp limit) to the bus. If a device is capable of supplying its own power, it must prioritize the use of its own power for internal operation where possible.

It is the responsibility of the power-supplying component to protect itself and its power-supplying circuitry from overcurrents in excess of what it is rated for. A self-recovering overcurrent protection, such as a polyfuse/self-healing fuse or eFuse (preferred) are required for this current limiting. Reference component for this application is the [MX25947ES](https://www.lcsc.com/product-detail/C5875698.html).

When a component implements an open-ended connection for an external supply that does not have a well-established voltage level, such as a barrel jack, the component must implement over- and under-voltage protection to avoid damage to the deployment. Reference component for this application is the [MX25947ES](https://www.lcsc.com/product-detail/C5875698.html).

Power suppliers that are not inherently powered by their normal use (i.e. requires an additional, external connection to power) but require that external power to operate properly must implement hardware to pull down the Interrupt pin when there is an invalid power connection. 

## Consumer Requirements

Power consuming components are required to implement a polyfuse/self-healing fuse on their input, to protect the deployment at-large from a short inside of that component. 

Power consumers can require the presence of the 12v rail before operating. If this is the case, the power consumer must implement hardware to pull down the Interrupt rail when there is no 12v power present. 

Components can assume the 5v rail is always present, since it is provided by the Core. The lack of a present 5v rail is considered an invalid state, and components are not expected to operate in this scenario. 

## Indicators

All components are required to have user-visible LED indicators of their power state. This LED must be green. The LED must be able to indicate that all requires power rails are present and working. An LED on each the 5v and 12v rail is permissible, or a single LED lit by the combination of the two.
