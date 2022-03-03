This is a sketch for the data that is required from the different layers towards the environmental data collection action to determine the environmental footprint.
Note that is only for the first iteration, in later iterations we will expand of course and include all lifecycle aspects (resource usage, etc.)

* From the server: 
  * Embodied carbon emissions (CO2-eq in kg) 
  * Real power consumption in kWh
* From the data center: 
  * Embodied carbon emissions (CO2-eq in kg) of all equipment + building
  * overhead power usage (PUE or actual overhead power attributed to the server in kWh)
  * Optional but useful: breaking down overhead power consumption of network and everything else (cooling, UPS loss, etc.)
* From the energy grid 
  * Carbon intensity of the current power supply (CO2-eq in kg per kWh)

## Using constants as fallback

For each of these external data requirement, we are defining a constant value, in case the actual value is not retrievable, to fall back to.

* SERVER_EMBODIED_CO2EQ = 2600 (kg of CO2-eq)
 * Using as a reference a [Dell Power Edge R640](https://i.dell.com/sites/csdocuments/CorpComm_Docs/en/carbon-footprint-poweredge-r640.pdf), without the carbon from power usage and assumed lifetime of 5 years; with a factor of 2x to cover also older server models
* SERVER_POWER_CONSUMPTION = 0.5 (kW per hour)
 * Using as a reference a [Dell Power Edge R640](https://i.dell.com/sites/csdocuments/CorpComm_Docs/en/carbon-footprint-poweredge-r640.pdf), with a 150% margin added for older servers and redudancy
* DATA_CENTER_PUE = 1.58 (58% overhead for 1 kW of installed server capacity)
 * Taking a conservative PUE assumption, based on the average calculation of the [Uptime Institute](https://journal.uptimeinstitute.com/data-center-pues-flat-since-2013/)
* ENERGY_GRID_CO2EQ_INTENSITY = 0.385553 (kg of CO2-eq)
 * Based on [US Grid/US Energy Information Administration](https://www.eia.gov/tools/faqs/faq.php?id=74&t=11)
