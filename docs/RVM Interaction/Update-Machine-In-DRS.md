---
stoplight-id: foryzcd3xzn6k
---

# Add or Update Machine In DRS by RVM Operator

![RVMConfigUpdate.png](../../assets/images/RVMActionInDRS.png)

# RVM Configuration Update

## Overview
This process allows **RVM providers** to add or update selected configuration parameters of the **RVM** directly in the **DRS** system.

## Process Flow
1. **Start:** The update process is initiated from the **RVM Cloud**.
2. **API Endpoint:** Configuration data is sent to the **DRS** system using the following endpoint `POST /machine/{id}`
3. **Response:** After the update is processed, **DRS** responds with a **confirmation**.

## Possible Updates by RVM Provider

The following parameters can be updated in the DRS system by the RVM provider:

1. **RVM Location**  
   Update the location where the RVM is installed.

2. **Bin Configuration**  
   Define or modify the number of bins in the RVM, their types (e.g., plastic, cans) and the target destination assigned to each bin.


<!--
type: tab
title: DRS
-->

Representation of API endpoint exposed by **DRS** for configuration update.

### POST /machine/{id}

For a full overview of this endpoint, please visit: [POST - /machine/{id}](https://kaucja.stoplight.io/docs/rvm-api/bu3ambgd8l19t-machine-update)

<details>
<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../drs-openapi.yaml#/components/schemas/MachineUpdate'
```
</details>

<!-- type: tab-end -->

---
<div style="text-align: right"> Version: 0.9.2</div>
