---
stoplight-id: 9yhko2vrj5xqa
---

![DepositCatalogueSync.png](../../assets/images/DepositCatalogueSync.png)

# Product Catalogue Sync Process

## Overview

The **Product Catalogue Sync** process ensures that product data is regularly updated between **RVM Cloud** and **DRS**. This is a mandatory process that must be performed **at least once every 24 hours** to maintain data consistency.

## Process Flow

1. **Request to RVM Cloud:** A request is initiated from **RVM Cloud** to **DRS**.
2. **API Endpoint:** The request is made to the `/product` endpoint in **DRS**.
3. **Response:** DRS processes the request and sends back a reply. Repeat until all records are sent.
4. **Data Sync Completion:** The response ensures that product data is correctly synchronized.

## Pagination

The `/product` endpoint is **paginated**. By default, the response contains **approximately 100 elements per page**. However, this number is **configurable by RVM** to meet specific data retrieval needs.

## API Endpoints

List of endpoints that should be exposed from **Deposit Return System (DRS)** and **RVM Cloud** in order to complete described process.

<!--
type: tab
title: DRS
-->
Representation of API Endpoinds exposed by **Deposit Return System (DRS)** in order to complete this prcess.

### GET /product

For full overwiev of this endpoint please visit: [GET /product](https://kaucja.stoplight.io/docs/rvm-api/i8big2em2kghv-get-product-data)

<details>
<summary>Response</summary>

```yaml jsonSchema
  $ref: '../../models/Product.yaml'
```

</details>

<!-- type: tab-end -->

---
<div style="text-align: right"> Version: 0.9</div>
