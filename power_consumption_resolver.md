## Resolver of power consumption

As it is incredibly difficult to consistently measure the power consumption of virtualized or physical server environments, we will use different layers of precision, depending on the availability.

The following list is ordered by higher precision to lower precision:

- Environmental data agent is able to receive power consumption from the power distribution unit (PDU, power plug) of the underlying server as well as the overhead power consumption from the surrounding infrastructure (data center cooling, etc.)
    - If overhead power consumption is not available, a constant is used of 1.6 (60% overhead)
    - If power consumption from the PDU is not available, software-based indicators are attempted, these can be (1) e.g. Intel RAPL data, (2) exposed APIs from the virtualization environment, or (3) APIs exposed by the underlying operating system (e.g. lf_perf)
        - If the power consumption of software indicators is unavailable or inaccurate, the underlying CPU or server hardware will be identified and the maximum rated power consumption will be queried from the SDIA’s open data hub and used as a constant
            - If the hardware-specific information can not be determined or there is no entry in the SDIA’s open data hub database, we revert to the final resort, using a constant, which represents an average of all known server power consumptions in the SDIA database.

The aim of this is to define our fallback logic, rather than the technical decisions on how and where to measure each of the data points.
