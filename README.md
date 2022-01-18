# An API & open standard for measuring carbon footprint for server-side software applications (SSAs)

This repository contains of two parts:

* An open standard specification for measuring & assembling the carbon footprint
* A REST API specification to expose the footprint within IT infrastructure (e.g. within a container or virtualization platform)

## Approach

When a server-side application is running, it is consuming resources - such as CPU time, memory and storage capacity. These resources are produced by physical IT infrastructure - physical servers sitting inside data centers or server rooms. The resources are often managed by virtualization or container orechestration platforms, or both. Each virtual resource unit is produced by a physical server, which comes with an embedded (e.g. from manufacturing or transport) and operational footprint (e.g. from the electricity needed to produce it). The server itself, also requires physical infrastructure, such as a building, electricity and cooling, in order to be able to produce resources. 

Therefore, each resource (CPU time, memory, storage) that a server-side application is consuming, comes with a measurable environmental & social costs which this project aims to expose to the application through an API within the IT infrastructure. The API must be available within a virtual machine, a container or a physical machine, making it easy & convient to access real-time carbon footprint of the resources consumed by the application.

## Scope

This specification focusses on assessing the environmental footprint of both physical IT infrastructure (servers, racks, etc.) and the supporting infrastructure (cooling, power conversion, backup power generation and storage, etc.) both from an operational and embedded perspective. It is aimed creating a realistic and true assessment of the carbon footprint of server-side applications.

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

## Carbon Footprint Specification

