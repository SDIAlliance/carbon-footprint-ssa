## Resource allocation solver

As the majority of the IT infrastructure environments are either virtualized or containerized, we must define a conversion formula for turning the resource consumption of each virtualized or containerized machine into power consumption. 

An example will illustrate the challenge. Assuming there is a physical server that is virtualized. It has 3 virtual machines. Further resource allocation is the following:

- Physical server: 128 GB Memory, 2 CPU Sockets (12 cores each), 24 cores total, 2 x 3.6 TB NVMe disks, 2 x 10 GbE Ethernet, total power consumption: 0.6 kWh
    - Virtual machine 1: 12% memory, 24% CPU, 3% storage, 2% network capacity
    - Virtual machine 2: 40% memory, 38% CPU, 12% storage, 18% network capacity
    - Virtual machine 3: 33% memory, 3% CPU, 24% storage, 8% network capacity
    - Idling resources (remainder of non-utilized workloads)
    - Overhead from virtualization/containerization itself
    - Operating system overhead

Now how to attribute the resource usage to power consumption?

### Background research

> According to (Minas and Ellison, 2012), the power used on servers is increasing and the two largest consumers of power are the processor and the memory. Otherwise, several research works try to optimize systems to reduce DRAM power consumption: (Kangetal.,2010);(HurandLin,2008); (Emmaetal.,2008); (Zhengetal.,2008); (Vogelsang,2010).
 
[Source](https://hal.archives-ouvertes.fr/hal-01314070/document)

> Our experiments show that background power dominates memory power consumption for in-memory databases, for both TPC-C and TPC-H workloads. Active power does increase with load, but it represented only 30% total memory power in the most intensive OLAP workload we tested, and did not exceed 10% in OLTP work- loads. Our results also show that memory power consumption is not reduced when memory is not fully used, i.e., when the database is small. 
> We believe that the implications of this are clear: memory power optimization for DBMS must focus on background power. That is, the goal must be to increase the amount of time that DIMMs spend in low power states, especially Self Refresh. Unfortunately, today’s systems, which are typically configured with interleaved memory and which employ memory controllers with conservative power policies, work against such optimizations. Providing DBMS with better control over memory configuration parameters that affect background power consumption will allow them to exploit memory power/performance tradeoffs to save energy.

[Source](https://mydataball.com/wp-content/uploads/2018/10/DAMON17-MemPower.pdf)

### First conclusion

For now, it seems that there is no reliable research on how to attribute 1 Watt of input power into a server to its various components. In the research found, the CPU seems to still makes up the majority of the power consumption as well as variance. However, most of the research found merely focussed on memory and CPU rather than including storage (SSD, HDD, NVMe) and GPUs in their comparisons.

For now therefore we settle on the following ratio’s:

| Resource utilization | Allocation % |
| --- | --- |
| CPU | 65% |
| Memory | 20% |
| Storage | 10% |
| Network | 5% |
| GPU | excluded |

### Second iteration

The ratio of allocating resource usage to power consumption are not the same across different systems.
In the first iteration, we defined our default model. This represents our fallback.

In order to improve the accuracy of the formula, it should be possible to parameterize it.
This means that if the CPU, type of memory, storage, etc. is known and we have a specific, verified measurement on the allocation, we can take that instead of using the default assumption.

This would require, that a tool exists, which given a standard usage scenario, can measure the resource allocation distribution on a computer or server system.
Second it would require that a public database exists from which those values can be pulled.

This is similiar to [spec.org](https://spec.org) ([example here](https://www.spec.org/power_ssj2008/results/res2020q1/power_ssj2008-20191125-01012.html)), however not about CPU power consumption, but all components of the system and their respective share of the total power consumption of the system in various usage scenarios. 
