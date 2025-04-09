---
internal: true
---
![RVMRegistration.png](../../assets/images/RVMRegistration.png)

# Machine Registration Process

## Overview
The **Machine Registration** process is a performed by **DRS** through their mobile application. It ensures that a new machine is properly registered in **RVM Cloud**.

Operation is done once per RVM machine. If machine change operator, will be replaced in Collection Point - machine needs to be deleted and then new one added.

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

For full overwiev of this endpoint please visit: [POST - /machine](../../rvm-openapi.yaml/paths/~1machine/post)

<details>
<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../rvm-openapi.yaml#/components/schemas/PostMachine'
```

</details>

<!-- type: tab-end -->