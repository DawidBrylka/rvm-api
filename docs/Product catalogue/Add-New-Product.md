---
stoplight-id: r2zehj4qe028o
tags: [Product, Catalog]
---
# Adding/ new product by introducer
![NewProductAdded.png](../../assets/images/NewProductAdded.png)

## Description
# Product Addition Process  

The process of adding a new product to the manufacturer's product catalog follows these steps:  

1. The process begins with the submission of a new product by the **Introducer**.  
2. The **Introducer** submits a `POST /product` request to the **Deposit Return System (DRS)**.
3. The **DRS** forwards the `POST /product` request to the **Reverse Vending Machine (RVM) Cloud**, which enables product recognition in RVMs.  
4. Upon successful processing, **RVM Cloud** returns an `OK` response to **DRS**.  
5. The **DRS** then sends the `OK` confirmation back to the **Introducer**, completing the process.  

This process ensures that new products are registered in the **DRS** and made recognizable by **Reverse Vending Machines (RVMs)** in no time.

> **Node:** In case of any issues, those won't be forwarded back to the Introducer, also DRS will not retry this call in case of unavailability of **RVM Cloud**.

> **Note:** This process is optional. Regardless of its execution, all **RVM Providers** are required to perform a **full product data synchronization every 24 hours** to ensure their product database remains up to date. 

## API Endpoints  

- [DRS API - GET /product](https://kaucja.stoplight.io/docs/rvm-api/drs-openapi.yaml/paths/~1product/get)  
- [RVM API - POST /product](https://kaucja.stoplight.io/docs/rvm-api/rvm-openapi.yaml/paths/~1product/post)  
