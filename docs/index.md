---
icon: lucide/rocket
---

# 0. Introduction

This documentation specifies the standards for MakeACS and MakeACS-compatible hardware, to ensure proper interoperability between components. 

## What is MakeACS?

MakeACS is an open-source set of hardware, software, and standards for a simple, versatile shop management and access control system. Originally developed at the Rochester Insitute of Technology, this system provides many automations to assist in the management and oversight of makerspaces, Fab Labs, teaching shops, etc. 

The *hardware* is primarily for machine-level access control, statistics collection, and shop automation. Users are permitted to only use equipment they are authorized to use (based on trainings, staff sign-offs, SSO credential information, etc.), and their usage (or failure to use) equipment is logged for statistical analysis, or to automatically inform things like maintenance tickets based on usage.

The *server* is where users, staff, and the hardware communicate. The server incorporates a training management and serving system for safety and competency trainings (as well as the ability to API-integrate with other services), centralized inventory, maintenance, usage, and staff tracking, and an MQTT API for the hardware deployed in the shop.

The hardware and server are both designed to be easy to adapt without the other, with well-defined, open-ended APIs for all interactions. 

This documentation will focus specifically on the hardware, but more information on MakeACS in general, or of a specific aspect of it, can be found in the [MakeACS GitHub](https://github.com/MakeACS).

## Terminology

* **Deployment** : All physically interconnected pieces of MakeACS hardware.
* **Component** : A single piece of MakeACS hardware.
* **Core** : The master of a MakeACS deployment. There is only one, and it serves as the primary point of communication out of the deployment.
* **Switch** : A MakeACS component intended to control access to a device, by interrupting its normal operation in some way.

## Topology

A MakeACS deployment can be made of one to many components, but the general topology is always the same;
* A deployment must exactly one Core.
* The Core always makes up one end of the MakeACS deployment's topology.
* Further MakeACS components can be connected to the Core via the standard interface. 
* Some MakeACS components are **passthrough**, meaning there is a standard interface on both sides to allow further connection of another device.
* Some MakeACS components are **terminating**, meaning there is only one standard interface to connect with.
* Some MakeACS components are **splitting**, meaning there is one input and multiple outputs, allowing the fanout of the deployment.
In most scenarios, the most common deployment is one Core connected to one Switch, sometimes with a passthrough component in the center. 

## Table of Contents

0. Introduction
1. 