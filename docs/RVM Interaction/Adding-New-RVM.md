![RVMRegistration.png](../../assets/images/RVMRegistration.png)

# Add/Update Machine 

## Overview

The **Add or update Machine** process is a performed by **DRS** through their mobile application. It ensures that a new machine is properly registered in **RVM Cloud**.


## Process Flow

1. **Start:** The registration process begins in the **DRS mobile app**.
2. **API Endpoint:** The request is made to the `/machine` endpoint in **RVM Cloud**.
3. **Response:** RVM Cloud processes the request and sends back a **confirmation**.

<!--
type: tab
title: RVM
-->

Representation of API Endpoinds exposed by **RVM Cloud** in order to complete this prcess.

### POST /product

For full overwiev of this endpoint please visit: [POST - /machine](https://kaucja.stoplight.io/docs/rvm-api/35nqny8k7q39d-add-new-rvm)

<details>
<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../rvm-openapi.yaml#/components/schemas/PostMachine'
```

</details>

<!-- type: tab-end -->

---
<div style="text-align: right"> Version: 0.9</div>
