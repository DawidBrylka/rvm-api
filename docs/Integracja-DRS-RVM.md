# Overview

This document is describing API expected by Kaucja.pl for contact between RVM and Kaucja.pl core DRS system.

Desired communcation format by Kacuja Pl looks like:

![InteractionModel.jpg](../assets/images/InteractionModel.jpg)

## Integration

This document defines two separate files, each specifying the endpoints that should be exposed on either side of the integration.

  * [DRS](../drs-openapi.yaml) This file defines the API that will be exposed by kaucja.pl. It handles communication and data exchange with external systems.

  * [RVM](../rvm-openapi.yaml) This file specifies the API that RVM providers should expose. This allows DRS to establish a connection and exchange information with RVM systems.

The purpose of this document is to clearly define how both systems will communicate, ensuring compatibility and seamless integration.



---
<div style="text-align: right"> Version: 0.9.1</div>
