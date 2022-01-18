# An API & open standard for measuring carbon footprint for server-side software applications (SSAs)

This repository contains the source code of the work on the specification for (1) calculating the carbon footprint of server-side applications and (2) an API reference on how to expose the footprint as an API within IT infrastructure software of the [Digital Carbon Footprint Steering Group](https://sdialliance.org/steering-groups/assessing-the-digital-carbon-footprint) (SG) of the [SDIA](https://sdialliance.org).

The repository include specifications, use cases and requirements, best practices and guidelines, primers and notes. The SG's Work Items are linked from the SG homepage.

This repository contains of two parts:

* An open standard specification for measuring & assembling the carbon footprint
* A REST API specification to expose the footprint within IT infrastructure (e.g. within a container or virtualization platform)

## Table of Contents

* [Approach](#approach)
* [Scope](#scope)
* [Technical Architecture](#architecture)
* [Carbon Footprint Open Standard](#footprint)
* [Participate](#participation)

<a name="approach"></a>

## Approach

When a server-side application is running, it is consuming resources - such as CPU time, memory and storage capacity. These resources are produced by physical IT infrastructure - physical servers sitting inside data centers or server rooms. The resources are often managed by virtualization or container orechestration platforms, or both. Each virtual resource unit is produced by a physical server, which comes with an embedded (e.g. from manufacturing or transport) and operational footprint (e.g. from the electricity needed to produce it). The server itself, also requires physical infrastructure, such as a building, electricity and cooling, in order to be able to produce resources. 

Therefore, each resource (CPU time, memory, storage) that a server-side application is consuming, comes with a measurable environmental & social costs which this project aims to expose to the application through an API within the IT infrastructure. The API must be available within a virtual machine, a container or a physical machine, making it easy & convient to access real-time carbon footprint of the resources consumed by the application.

<a name="scope"></a>

## Scope

This specification focusses on assessing the environmental footprint of both physical IT infrastructure (servers, racks, etc.) and the supporting infrastructure (cooling, power conversion, backup power generation and storage, etc.) both from an operational and embedded perspective. It is aimed creating a realistic and true assessment of the carbon footprint of server-side applications.

<a name="architecture"></a>

## Technical Architecture 

_For those who prefer a more visual view, please take a look at this [Miro board](https://miro.com/app/board/uXjVOalAbwM=/)._

### IT Infrastructure APIs

Calculating the carbon footprint of a running application either continously or over a time period should be simple and convient for the application developer in order to increase adoption. Therefore we are creating sub-groups which will implement the API specification into various container orchestration and virtualization platforms, starting with Kubernetes and OpenStack.

You can find the respective repositories here:

* CF-K8: https://github.com/sdialliance/carbon-footprint-kubernetes-api
* CF-OS: https://github.com/sdialliance/carbon-footprint-openstack-api

### Carbon Footprint Data Agent (CF-Agent)

To be able to calculate an accurate carbon footprint of the physicial infrastructure (from servers to data centers), data must be collected and be made available to the IT Infrastructure APIs. The Carbon Footprint Data Agent (CF-Agent) or any environmental assessment/monitoring system implementing its API runs within the physical infrastructure, connect & collects data from relevant systems, enriches it and provides the data source APIs required to calculate the footprint of the application within the IT Infrastructure APIs.

* CF-Agent API specification: https://github.com/sdialliance/carbon-footprint-agent-api
* CF-Agent implementation: https://github.com/sdialliance/carbon-footprint-agent

### Open Data Hub (ODH)

The open data hub, operated as a public data repository by the [SDIA](https://sdia.io) is used by the CF-Agent to enrich the collected information from the physical infrastructure. As an example, the ODH contains a database of server hardware and its embedded emissions. It also provides real-time APIs for the CO2-concentration of electricity for different locations.

* ODH API specification: https://github.com/sdialliance/carbon-footprint-agent-api

<a name="footprint"></a>

## Calculating the Carbon Footprint

In order to ensure that the carbon footprints are calculated using the same methodology across different environments, we are working on an open standard which can be continously improved as we learn more about the footprint of software. The standard is developed by the members of the SG as part of a monthly meeting. Anyone can contribute, please see participation below.

<a name="participation"></a>

## Participation

All substantive contributors must be members of the SDIA SG. It’s easy to [join the SG](https://sdialliance.org/steering-groups/assessing-the-digital-carbon-footprint) if you’d like to contribute.

Anyone can open [issues](https://github.com/SDIAlliance/carbon-footprint-ssa/issues) and contribute to this repository.

## References

* [ICT scope standards (1450)](https://www.itu.int/rec/T-REC-L.1450-201809-I/en)
* [ICT scope standards (1470)](https://www.itu.int/rec/T-REC-L.1470/en)
* [ICT scope standards (1410)](https://www.itu.int/rec/T-REC-L.1410/en)
* [Green Cloud Computing - German Environmental Protection Agency](https://www.umweltbundesamt.de/sites/default/files/medien/5750/publikationen/* 2021-06-17_texte_94-2021_green-cloud-computing.pdf)
* [Guidance on GHG Protocol](https://ghgprotocol.org/guidance-built-ghg-protocol)
* [GHG Protocol](https://www.ghgprotocol.org/sites/default/files/ghgp/GHGP-ICTSG%20-%20ALL%20Chapters.pdf)