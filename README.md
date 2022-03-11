# An API & open standard for measuring carbon footprint for server-side software applications (SSAs)

This repository contains the source code of the work on the specification for (1) calculating the carbon footprint of server-side applications and (2) an API reference on how to expose the footprint as an API within IT infrastructure software of the [Digital Carbon Footprint Steering Group][1] (SG) of the [SDIA][2].

[Read more about our approach on our blog.](https://blog.sdialliance.org/steering-group-update-environmental-footprint-framework-for-server-side-applications)

The repository include specifications, use cases and requirements, best practices and guidelines, primers and notes. The SG's Work Items are linked from the SG homepage.

This repository contains of two parts:

* An open standard specification for measuring & assembling the carbon footprint
* A REST API specification to expose the footprint within IT infrastructure (e.g. within a container or virtualization platform)

## Table of Contents

* [Approach][3]
* [Scope][4]
* [Technical Architecture][5]
* [Carbon Footprint Open Standard][6]
* [Participate][7]

<a name="approach"></a>

## Approach & Architecture

When a server-side application is running, it is consuming resources - such as CPU time, memory and storage capacity. These resources are produced by physical IT infrastructure - physical servers sitting inside data centers or server rooms. The resources are often managed by virtualization or container orechestration platforms, or both. Each virtual resource unit is produced by a physical server, which comes with an embedded (e.g. from manufacturing or transport) and operational footprint (e.g. from the electricity needed to produce it). The server itself, also requires physical infrastructure, such as a building, electricity and cooling, in order to be able to produce resources. 

Therefore, each resource (CPU time, memory, storage) that a server-side application is consuming, comes with a measurable environmental & social costs which this project aims to expose to the application through an API within the IT infrastructure. 

The challenge now is to bring together a virtual machine, or more bleeding-edge, a Kubernetes pod, with the actual power consumption of the server. By design, one is not supposed to access the physical server from within a pod, or a virtual machine. It’s the abstraction provided by the system.

Therefore we have chosen (for now) to build an architecture around the [Observer][8] pattern, whereby each part of the system reports its relevant metrics to an Agent (the observer), which, using the collected information, in turn provides each part with a the information on the actual power consumption.

![Architecture Diagram](https://docs.google.com/drawings/d/e/2PACX-1vTL2svMiZCM1z1DRSv7A90_qCyT8Q6bvcAcuroxOly8hgRxfzdIKS6t-CVSAK7sEc1kWs549degfBjk/pub?w=1552&amp;h=843)

The observer (The Environmental Footprint Data Agent - EFD-Agent) thus records three types of time-series through observation:

* A time-series of [power consumption](https://github.com/SDIAlliance/carbon-footprint-ssa/blob/main/power_consumption_resolver.md) (kW per second)
* A time-series of resource utilization (CPU, memory, storage & network capacity) of a pod or virtual machine
* A time-series of the schedule on which virtual machines and pods where running (e.g. recording when they started and stopped)

Combining these three time series with a [solver that attributes the different resource types to the power consumption](https://github.com/SDIAlliance/carbon-footprint-ssa/blob/main/resource_allocation_resolver.md) (e.g. 60% to CPU, 20% to memory, 10% to storage, 10% to the network) then leads us to be able to determine how much power a pod or virtual machine was consuming, based on it’s resource use, while it was running. Even better: It allows us to measure the opposite as well - how much power was wasted on idling/non-use of available resources.

* [The solver is described further in this document](https://github.com/SDIAlliance/carbon-footprint-ssa/blob/main/resource_allocation_resolver.md)
* [Our approach & fallback to power consumption measurement is here](https://github.com/SDIAlliance/carbon-footprint-ssa/blob/main/power_consumption_resolver.md)

The API specification in this repository defines the APIs necessary within the EFD Agent:

* Kubernetes Pod Registry API - to record start/stop and run times of each pod
* Kubernetes Pod Resource API - to record the resource allocation/usage of each pod
* Server Power Consumption API - to record the power consumption of the server

\# TODO: Add the actual links to the API specs.

### Where does the EFD-Agent run?

Of course, given the virtualized nature of today’s IT infrastructure, the EFD Agent itself can be running within the IT infrastructure that is being monitored itself. Meaning it can be deployed within the same Kubernetes cluster that it observes. 

This also means that the EFD-Agent is being observed itself - by itself and can determine its power consumption - which should be considered overhead and to work towards minimizing this overhead.

The role of the EFD-Agent can also be assumed by existing systems, such as the virtualization layer (e.g. OpenStack or VMWare) or it can be assumed by the physical infrastructure (e.g. the Data Center Infrastructure Management System - DCIM). As long as the physical server, the pods or virtual machines are able to reach the agent, it’s actual location is not important.

<a name="scope"></a>

## Scope

This specification focusses on assessing the environmental footprint of both physical IT infrastructure (servers, racks, etc.) and the supporting infrastructure (cooling, power conversion, backup power generation and storage, etc.) both from an operational and embedded perspective. It is aimed creating a realistic and true assessment of the carbon footprint of server-side applications.

In the first version, we have decided to **focus on the server itself**, while using constants/pre-defined values for most of the infrastructure. Therefore we focus on measuring two values only at the moment:

* The physical power consumption of the server
* The embedded carbon of the server from transport & manufacturing

<a name="architecture"></a>

## Additional data sources

### Open Data Hub (ODH)

The open data hub, operated as a public data repository by the [SDIA][9] is used by the CF-Agent to enrich the collected information from the physical infrastructure. As an example, the ODH contains a database of server hardware and its embedded emissions. It also provides real-time APIs for the CO2-concentration of electricity for different locations.

* ODH API specification: https://github.com/sdialliance/carbon-footprint-agent-api

<a name="footprint"></a>

## Calculating the Environmental Footprint

In order to ensure that the carbon footprints are calculated using the same methodology across different environments, we are working on an open standard which can be continously improved as we learn more about the footprint of software. The standard is developed by the members of the SG as part of a monthly meeting. Anyone can contribute, please see participation below.

### Drafts

For the time being, we have decided to focus on the environmental footprint, specifically energy-use and embodied carbon of servers.

As the precision of data available to convert electrical power into carbon emissions (e.g. based on values reported by the grid) is very low, we find it more useful to simply measure the power consumption itself, instead of using carbon emissions as a potentially misleading proxy.

* [0.1.0 Draft Environmental Footprint][10]

<a name="participation"></a>

## Participation

All substantive contributors must be members of the SDIA SG. It’s easy to [join the SG][11] if you’d like to contribute.

Anyone can open [issues][12] and contribute to this repository.

## References

* [ICT scope standards (1450)][13]
* [ICT scope standards (1470)][14]
* [ICT scope standards (1410)][15]
* [Green Cloud Computing - German Environmental Protection Agency][16]
* [Guidance on GHG Protocol][17]
* [GHG Protocol][18]

[1]:	https://sdialliance.org/steering-groups/assessing-the-digital-carbon-footprint
[2]:	https://sdialliance.org
[3]:	#approach
[4]:	#scope
[5]:	#architecture
[6]:	#footprint
[7]:	#participation
[8]:	https://en.wikipedia.org/wiki/Observer_pattern
[9]:	https://sdia.io
[10]:	https://github.com/SDIAlliance/carbon-footprint-ssa/blob/main/environmental_footprint.v0.1.0.md
[11]:	https://sdialliance.org/steering-groups/assessing-the-digital-carbon-footprint
[12]:	https://github.com/SDIAlliance/carbon-footprint-ssa/issues
[13]:	https://www.itu.int/rec/T-REC-L.1450-201809-I/en
[14]:	https://www.itu.int/rec/T-REC-L.1470/en
[15]:	https://www.itu.int/rec/T-REC-L.1410/en
[16]:	https://www.umweltbundesamt.de/sites/default/files/medien/5750/publikationen/2021-06-17_texte_94-2021_green-cloud-computing.pdf
[17]:	https://ghgprotocol.org/guidance-built-ghg-protocol
[18]:	https://www.ghgprotocol.org/sites/default/files/ghgp/GHGP-ICTSG%20-%20ALL%20Chapters.pdf
