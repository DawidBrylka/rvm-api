---
stoplight-id: rvm-noscan-xyz789
---

![RVMWithoutScanner.png](../../assets/images/RVMNoScanner.png)

# RVM Without Scanner

## Overview

For **RVMs that do not have an internal scanner**, the process of scanning the seal is handled via a **mobile application** provided by the DRS Operator or an equivalent device.

> **RVM** must only be unblocked after receiving confirmation of sealing from **DRS**.

## Process Flow

### Case 1 – Full Bag Notification Present

1. **Start:** The RVM detects that the bag is full.
2. **API Endpoint:** RVM Cloud sends a notification to DRS using `POST /bag-is-full`
3. **Propagation:** DRS informs Collection Point about the full bag.
4. **Bag Replacement:** Once the bag is emptied, **RVM Cloud** notifies **DRS** via `POST /bag-replacement`
5. **Seal Request:** DRS requests a seal from PZ.
6. **Seal Confirmation:** Collection Point confirms **DRS** sealing `POST /bag-seal`
7. **Machine Unblock:** DRS sends a command to RVM Cloud to unblock the machine `POST /machine/{id}/unblock`

### Case 2 – Proactive bag replacement (Bag is not full yet)

1. **Start:** Bag replacement is performed manually.
2. **Bag Replacement:** RVM Cloud informs DRS via `POST /bag-replacement`
3. **Seal Request:** DRS notifies PZ that a seal is needed.
4. **Seal Confirmation:** PZ sends confirmation of sealing `POST /bag-seal`
5. **Machine Unblock:** DRS unblocks the RVM -  `POST /machine/{id}/unblock`

## API Endpoints

List of endpoints that should be exposed from **Deposit Return System (DRS)** and **RVM Cloud** in order to complete described process.

<!--
type: tab
title: DRS
-->

### POST /bag-seal

Sent by **Collection Point** via Mobile App to **DRS** to confirm sealing of the bag.

For full overwiev of this endpoint please visit: [POST - /bag-seal](../../drs-openapi.yaml/paths/\~1bag-seal/post)

<details>

<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../drs-openapi.yaml#/components/schemas/BagSeal'
```

</details>
<br>

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

<!--
type: tab
title: RVM
-->

### POST /machine/{id}/unblock

Sent by DRS to unblock the RVM after sealing is confirmed.

For full overwiev of this endpoint please visit: [PUT - /machine/{id}/unblock](../../rvm-openapi.yaml/paths/\~1machine\~1{id}\~1unblock/put)

<details>
<summary>Path Parameter</summary>

```yaml
id:
  type: string
  description: Unique identifier of the machine.
```

</details>

<!-- type: tab-end -->
