---
stoplight-id: rvm-noscan-xyz789
---

![RVMWithoutScanner.png](../../assets/images/RVMNoScanner.png)

# RVM Without Scanner

## Overview

For **RVMs that do not have an internal scanner**, the process of scanning the seal is handled via a **mobile application** provided by the DRS Operator or an equivalent device.

> **RVM** must only be unblocked after receiving confirmation of sealing from **DRS**.

## Process Flow

We do expect to follow one of available cases, one of th

### Case 1 – Bag Fill Status Notification Present

This flow applies to RVMs that support **bag fill level detection** and notify DRS when the bag is filling up or full.

1. **Start:** The RVM detects the bag has reached a certain fill threshold.
2. **Bag Fill Notification:** RVM Cloud sends fill status to DRS via `POST /bag-fill-status`. 
3. **Notification Propagation:** DRS notifies the Collection Point Warning about filled bag.
4. **Full Bag Notification:** When the bag is full, RVM Cloud sends a another `POST /bag-fill-status` request - now RVM should lock itself until seal is applied.
5. **Notification Propagation:** DRS notifies the Collection Point about the full bag.
6. **Bag Replacement:** After the bag is emptied, RVM Cloud sends `POST /bag-replacement` to DRS.
7. **Seal Request:** DRS requests the Collection Point to confirm sealing.
8. **Seal Confirmation:** Collection Point sends confirmation to DRS via `POST /bag-seal`.
9. **Machine Unblock:** Once sealing is confirmed, DRS sends `POST /machine/{id}/unblock` to RVM Cloud.

> In case single transaction have filled from normal status to full, warning should not be sent. Only single call that informs that bag is full.

---

### Case 2 – No Bag Fill Status Notification

This flow applies when:

- The RVM **does not support bag fill status detection**, **or**
- The bag was already replaced **before** any notification could be sent

1. **Start:** The bag is replaced manually (proactively or without system detection).
2. **Bag Replacement:** RVM Cloud informs DRS via `POST /bag-replacement`. At this point RVM should be blocked until Seal is applied
3. **Seal Request:** DRS notifies the Collection Point that sealing is required.
4. **Seal Confirmation:** Collection Point confirms sealing via `POST /bag-seal`.
5. **Machine Unblock:** DRS sends a command to RVM Cloud to unblock the machine using `POST /machine/{id}/unblock`.



## API Endpoints

List of endpoints that should be exposed from **Deposit Return System (DRS)** and **RVM Cloud** in order to complete described process.

<!--
type: tab
title: DRS
-->

### POST /bag-seal

Sent by **Collection Point** via Mobile App to **DRS** to confirm sealing of the bag.

For full overwiev of this endpoint please visit: [POST - /bag-seal](https://kaucja.stoplight.io/docs/rvm-api/lqz2mv777xppe-call-performed-when-bag-seal-is-performed)

<details>

<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../drs-openapi.yaml#/components/schemas/BagSeal'
```

</details>
<br>

### POST /bag-is-full

Informs DRS about the RVM's bag fill level. 

For full overwiev of this endpoint please visit: [POST - /bag-fill-status](https://kaucja.stoplight.io/docs/rvm-api/bd2d38b8c5dc3-notify-about-bag-fill-level)

<details>

<summary>Request Body</summary>

```yaml jsonSchema
  $ref: '../../drs-openapi.yaml#/components/schemas/BagFillStatus'
```

</details>
<br>

<br>

### POST /bag-replacement

Informs DRS that a bag has been replaced.

For full overwiev of this endpoint please visit: [POST - /bag-replacement](https://kaucja.stoplight.io/docs/rvm-api/3r55dg8tllqbx-trigger-an-replacement-action-for-rvm)

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

For full overwiev of this endpoint please visit: [PUT - /machine/{id}/unblock](https://kaucja.stoplight.io/docs/rvm-api/qodfbkmaqt7j5-unblock-rvm-to-working-state)

<details>
<summary>Path Parameter</summary>

```yaml
id:
  type: string
  description: Unique identifier of the machine.
```

</details>

<!-- type: tab-end -->

---
<div style="text-align: right"> Version: 0.9</div>
