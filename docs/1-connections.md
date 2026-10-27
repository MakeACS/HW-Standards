---
icon: material/cable-data
---

# Connections

## Component Connector

The standard interface connector is the most important aspect of a MakeACS component, and is what ensures interoperability with all other components in a deployment. 

The MakeACS standard connector is an **HD15**, with components always having the female receptacle side. To prevent confusion with VGA or SVGA connections, blue receptacles, housings, cables, wiring harnesses, etc. are not permitted in a MakeACS deployment. 

!!! warning
    The HD15 used by MakeACS is the same physical connector as a VGA or SVGA video signal, but **plugging in a video cable to the MakeACS bus can damage the computer or monitor!**

## Component Interconnect

For connecting components, high-quality HD15 cables are to be used. The suggested ones for use with MakeACS are those manufactured by [Amphenol Cables on Demand](https://cablesondemand.com/hd15). Cables must meet the following criteria;
* Length of no more than 25 feet.
* Fully shielded, with shield connections to both connectors.
* Positive retention screws on both sides of both connectors.
* Wire gauge of 26 AWG or larger on all conductors. 
* No combined conductors.

## Wiring

### Power

See [power](./2-power.md) for more information.

**Ground (3, 10, 11, 14)** : Primary system ground, all power and signal is referenced to this ground level.

**12 Volt Bus (4, 5)** : Nominal 12v power for component use. Designs should be able to handle sustained 2 amps.

**5 Volt Bus (6, 13)** : Nominal 5v power for component use. Designs should be able to handle sustained 2 amps.

### Standard Signals

**Access (1)** : Digital signal, originates at the Core. 5v logic level, 30mA nominal drive strength, push-pull. This signal is the primary logical signal to indicate if a deployment should grant access to its equipment. Components that make use of this signal must have a user-visible LED that indicates when the signal is present, for debug purposes. This LED must be yellow. 

**OneWire (2)** : Open-drain bidirectional digital signal, pulled to 5v by the Core. 1mA nominal pull-up strength. This bus is used for enumeration and identification of components in a deployment, and informs the operation of the Core. See [Component ID](3-component-id.md) for more information.

**Interrupt (7)** : Open-drain bidirectional digital signal, pulled to 5v by the Core. 5mA nominal pull-up strength. This signal can be pulled low by any component to indicate that the deployment should be put into a suspended state (i.e. revoke access, do not approve access). The Core has the ability to pull this low as well, as a way to inform other components that the deployment is currently locked out or otherwise unavailable. Components capable of generating an Interrupt signal must have a user-visible LED that indicates the device is in an interrupt state. This LED must be red.

### Application-Defined Signals

See [GPIO](./4-gpio) for more information.

**GPIO 1 (8)** : Application-defined input/output #1. 

**GPIO 2 (9)** : Application-defined input/output #2. 

**GPIO 3 (12)** : Application-defined input/output #3. 

**GPIO 4 (15)** : Application-defined input/output #4. 

### Shield

The shield of all cables must be connected together with a large, continuous copper connection within each component that has more than one HD15 connector. All components leave the shield floating except for the Core, which ties it to ground for shielding and EMI purposes.

## Reduced Connector Standard (MakeACS-Lite)

For components that do not need or cannot fit the standard HD15 connector, and to allow for backwards-compatibility with the previous [SMACS Hardware Standard](https://github.com/rit-construct-makerspace/access-control-hardware), a reduced-pin interface is also standardized. Signals have the same voltage level and drive characteristics as they would in the HD15 connector. The DB9 shield is treated the same as the HD15 shield (i.e. grounded at the Core, floating otherwise).

1. Access
2. OneWire
3. Ground
4. 5 Volt Power
5. Ground
6. 5 Volt Power
7. Interrupt
8. Unused (Float, RFU)
9. Ground

!!! warning
    SMACS-compliant hardware will likely have issues related to power when used with the MakeACS Reduced Connector Standard, for the following reasons;

    * SMACS has a Schottky diode inline with each 5v power output pin, making effective bus power closer to 4.3-4.5v.
    * SMACS 5v rail is limited at 500mA maximum current, compared to MakeACS 1000mA minimum current.
    * SMACS does not connect pin 9 of the DB9, resulting in higher voltage losses across the line.
    * SMACS uses pin 6 as a "Type" identifier", resulting in much higher voltage drop across the line.

    When using a SMACS component with a MakeACS Core or similar, supplementary power for the Core may be required.

The pinout of the Reduced Connector Standard is chosen such that off-the-shelf HD15 to DB9 adapters, generally meant for CGA or MultiSync to VGA conversion, will properly adapt between the connections. 

## Permissible Deviations

### Application-Specific Connectors

Deviations from the connector standard are permitted where it makes for a more reliable, easier-to-use, or much less expensive component. However, a means to convert the component's deviation back to a standard HD15 must always be included. A common example is to have DIN rail-mounted switch that has bare screw terminals to connect to the MakeACS bus, so an HD15 to bare-wire cable can be more easily passed through the electrical box's cable glands for installation.

### Bridging Deployments

There are situations where connecting two deployments is necessary. When this happens, a dedicated interconnecting component must be used, that arbitrates the signals. Only the ground, power rails, and interrupt lines may be connected across the deployments directly. 