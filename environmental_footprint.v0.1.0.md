# Environmental footprint of a server-side applications

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## Metrics

The boundaries of these metrics are always the underlying physical server.
Thus in the following metrics 'the server' refers to this physical machine. 

- **Total power consumption** of the server
- **Total embodied carbon emissions** of the server

## Measuring the total power consumption

TODO

## Measuring the total embodied carbon 

TODO

* The embodied carbon emissions MUST include emissions from transport and manufacturing (reference standard needed)

## Assessing the total environmental footprint

* The server MUST record its power consumption while running on a contious basis, which SHOULD be at least on an minutely granularity
* The server SHOULD expose its own embodied carbon emissions via an API
  * If this is not available, a reference value SHOULD be used (yet to be defined)

## References

* [Draft article/publication on the environmental footprint of server-side software ](https://www.notion.so/sdia/Article-IT-Footprint-Server-Power-Consumption-Embedded-Carbon-Data-Center-overhead-1e2e7141a01c47238fbab51e3cf2f5a3)
