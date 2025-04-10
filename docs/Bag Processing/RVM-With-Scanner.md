---
stoplight-id: rvm-withscanner-abc123
---

![RVMWithScanner.png](../../assets/images/RVMWithScanner.png)

# RVM With Scanner

## Overview

The process for **RVMs equipped with a scanner** allows for the automated scanning of seals and immediate transfer of this information to the **DRS Operator**.

The process can be triggered either by:

- A notification from the RVM indicating that the bag is full
- A manual bag replacement initiated by a **Collection Point** staff member.

## Process Flow

### Case 1 – Full Bag Notification Present

1. **Start:** The RVM detects a full bag.
2. **API Endpoint:** RVM Cloud informs DRS `POST /bag-is-full`
3. **Propagation:** DRS informs PZ about the full bag.
4. **Bag Replacement:** Once the bag is replaced, RVM Cloud notifies DRS `POST /bag-replacement`

### Case 2 – No Initial Bag Full Notification

1. **Start:** A manual bag replacement is initiated.
2. **Bag Replacement:** RVM Cloud notifies DRS `POST /bag-replacement`

> **Note:** The internall scanner within **RVM** handles the seal scanning and transmits this data automatically during the process.

<!--
type: tab
title: DRS
-->

### POST /bag-is-full

Informs DRS that the RVM's bag is full.

For full overwiev of this endpoint please visit: [POST - /bag-is-full](../../drs-openapi.yaml/paths/\~1bag-is-full/post)

<details>

<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../drs-openapi.yaml#/components/schemas/BagFull'
```

</details>
<br>

<br>

### POST /bag-replacement

Informs DRS that a bag has been replaced.

For full overwiev of this endpoint please visit: [POST - /bag-replacement](../../drs-openapi.yaml/paths/\~1bag-replacement/post)

<details>

<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../drs-openapi.yaml#/components/schemas/BagReplacement'
```

</details>
<br>

<!-- type: tab-end -->
