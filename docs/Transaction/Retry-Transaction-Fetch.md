---
stoplight-id: lostsync-abc123
---

![LostDataSync.png](../../assets/images/LostDataSync.png)

# Lost Data Sync

## Overview

The **Lost Data Sync** process ensures that any transactions not marked as delivered in **RVM Cloud** are identified and confirmed by **DRS**. This mechanism helps maintain data consistency in case of transmission failures or missed confirmations.

## Process Flow

1. **Start:** The sync process is triggered on the **DRS** side (e.g., once daily).
2. **Fetch Unconfirmed Transactions:** DRS calls the following endpoint on **RVM Cloud**:
   - `GET /transaction?delivered=false`
   - This returns a list of transactions that have not yet been confirmed as delivered.
3. **Process Response:** DRS receives a list of transaction IDs not marked as delivered.
4. **Send Confirmations:** For each transaction ID, DRS sends a confirmation to **RVM Cloud**:
   - `POST /transaction/{id}/confirmation`

> **Note:** This process complements the standard transaction submission workflow. It acts as a recovery step to handle edge cases where confirmation was not properly recorded.

> **Note:** This is separate from the standard retry mechanism initiated by **RVM Cloud** upon submission failure.

<!--
type: tab
title: RVM
-->

Representation of API endpoints exposed by **RVM Cloud** to support this process.

### GET /transaction?delivered=false

Returns all transactions not yet marked as delivered.
For a full overview of this endpoint, please visit: [GET - /transaction](https://kaucja.stoplight.io/docs/rvm-api/9jlaedwrqzo5n-get-all-transactions)

<details>
<summary>Query Parameters</summary>

```yaml
delivered:
  type: boolean
  description: Filter to only return transactions that are not marked as delivered.
```

</details>

<details>
<summary>Response</summary>

```yaml jsonSchema
  $ref: '../../rvm-openapi.yaml#/components/schemas/PaginatedTransaction'
```

</details>

<br><br>

### POST /transaction/{id}/confirmation

Confirms delivery of a single transaction by ID.
For a full overview of this endpoint, please visit: [POST - /transaction/{id}/confirmation](https://kaucja.stoplight.io/docs/rvm-api/sw1i2ci6d2hgr-transaction-confirmation)

<details>
<summary>Path Parameter</summary>

```yaml
id:
  type: string
  description: Unique identifier of the transaction.
```

</details>

<!-- type: tab-end -->

---
<div style="text-align: right"> Version: 0.9.1</div>
