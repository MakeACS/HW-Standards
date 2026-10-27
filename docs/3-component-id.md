---
icon: lucide/id-card
---

# 3. Component ID

All MakeACS components except for the Core are required to implement a OneWire device on the OneWire pin of the bus. The reference device for this purpose is the [UMW DS18B20Z](https://www.lcsc.com/product-detail/C695837.html?s_z=n_q_C695837). 

This OneWire device is used for 3 main purposes;

* It provides a globally unique identifier for the component, which is used both for bus integrity checks (see [Unique Identifier](#unique-identifier)) and communicative device enumeration (see [GPIO](./4-gpio.md)).
* It provides a device type and classification to inform the Core how to operate (see [Device Classification](#device-classification)).
* It provides regular temperature monitoring of the deployment to look for anomalous conditions.

* Generic Device
* Generic Device II
* Communicative Device
* 2Ch Device
* 3Ch Device
* 4Ch Device

## Memory Structure

The DS18B20 and similar OneWire temperature sensors have a high-temperature and low-temperature threshold that is stored in persistent EEPROM. This EEPROM is repurposed on MakeACS in the following schema;

| Byte   | Bit | Purpose   |
|--------|-----|-----------|
| T-HIGH | 7   | CLASS 2   |
| T-HIGH | 6   | CLASS 1   |
| T-HIGH | 5   | CLASS 0   |
| T-HIGH | 4   | UNIQUE 12 |
| T-HIGH | 3   | UNIQUE 11 |
| T-HIGH | 2   | UNIQUE 10 |
| T-HIGH | 1   | UNIQUE 9  |
| T-HIGH | 0   | UNIQUE 8  |
| T-LOW  | 7   | UNIQUE 7  |
| T-LOW  | 6   | UNIQUE 6  |
| T-LOW  | 5   | UNIQUE 5  |
| T-LOW  | 4   | UNIQUE 4  |
| T-LOW  | 3   | UNIQUE 3  |
| T-LOW  | 2   | UNIQUE 2  |
| T-LOW  | 1   | UNIQUE 1  |
| T-LOW  | 0   | UNIQUE 0  |

See [Device Classification](#device-classification) for information on the 3 "CLASS" bits.

See [Type Identifier](#type-identifier) for information on the 13 "UNIQUE" bits.

## Device Classification

To inform the Core as to what types of the components make up a deployment, every component is given a classification. The classification is one of 8 options, as seen in the following table;

| Bits | Role            |
|------|-----------------|
| 000  | Communicative   |
| 001  | General Purpose |
| 010  | General Purpose |
| 011  | General Purpose |
| 100  | General Purpose |
| 101  | 2 Channel       |
| 110  | 3 Channel       |
| 111  | 4 Channel       |

**Communicative** components can interface with the Core via a higher-level communication protocol. See [GPIO](./4-gpio.md) for more information.

**Generic Components** are generic components that do not take advantage of the GPIO. These may be 1-channel Switches (switched directly off the Access signal), passive devices that just monitor the bus (like a status light), or a monitoring device that asserts the Interrupt (like a temperature sensor). 

**2, 3, and 4 Channel** are Switches that use the GPIO to switch multiple outputs, i.e. a 4 Channel Switch has 4 outputs, and when Access and GPIO 1 are both high, output 1 is active. See [GPIO](./4-gpio.md) for more information.

## Type Identifier

In addition to the 3 bit Device Classification, every device has a 13 bit Type Identifier. The Type Identifier allows for the Core to identify the model/series of component it is connected to, by referencing the [MakeACS Components Table](https://github.com/MakeACS/HW-Components-Table). 

Type Identifiers are issued sequentially in each Device Classification.

## Unique Identifier

OneWire devices inherently have a globally unique 64-bit identifier, which is used by MakeACS to verify the composition of the deployment. 

The Core will monitor for the presence of the expected Unique Identifiers, and if one is missing, or an unexpected one is found, it raises a fault condition until cleared by an authorized user, indicating to re-seal the deployment as-is.