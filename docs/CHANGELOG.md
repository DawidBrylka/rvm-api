---
stoplight-id: qlcl5lzd2yxfb
---

# CHANGELOG

## 20-11-2025

### Changed

- **Add / update machine**  
  - In [POST - /machine](https://kaucja.stoplight.io/docs/rvm-api/bu3ambgd8l19t-machine-update):  
  - Clarified `id` (`string<uuid>`) as the machine identifier used later in transactions and bag closing (`rvmId`).
  - **Marked `drsCpNo` as a required field.** 
  - Clarified the format of `serialNumber`.
  - Updated `name` description to note that customers will see this value in the DRS portal.  
  - Added `id` to the `200` response example.  
  - In [DELETE - /machine/{id}](https://kaucja.stoplight.io/docs/rvm-api/bu3ambgd8l19t1-machine-delete), clarified the origin of `id`.

- **Transactions**  
  - Updated [Transaction process](https://kaucja.stoplight.io/docs/rvm-api/nb5tsjecxrsga-transaction-process) to clarify that transactions via this endpoint can be sent only for RVM Stands created via the API.  
  - Further clarified field descriptions in [POST - /transaction](https://kaucja.stoplight.io/docs/rvm-api/sbyw8qv8u7gj8-post-single-transaction):  
    - `id` – explicitly described as the transaction ID.  
    - `rvmId` – described as the machine id obtained from `POST /machine`.  
    - `totalWeight` – confirmed that the value should be provided in grams.  
    - `voucher.redeemed` – clarified that it can also be `true` when the RVM/POS pays out the refund immediately (e.g. card payout / discount).  
    - `voucherId` – documented that leaving this field empty results in the voucher data being returned in the response.

- **Bag replacement**  
  - Updated [POST - /bag-replacement](https://kaucja.stoplight.io/docs/rvm-api/3r55dg8tllqbx-trigger-an-replacement-action-for-rvm):  
    - Clarified that `bagId` (UUID) is chosen by the sender and is visible to the client in the DRS system.  
    - Clarified that `rvmId` comes from machine registration (`POST /machine`).    
    - Documented that providing `relatedTransactionsIds` makes these transactions visible to the client in the closed bag view. **Providing these transaction lists is prefered by Kaucja.pl.**

- **Product data**  
  - Renamed [Get product data](https://kaucja.stoplight.io/docs/rvm-api/5o6n5p23xf20f-get-product-data) to **“Get data for a single product”** to distinguish it from the list endpoint.  
  - For [GET - /product](https://drs-sandbox.kaucja.pl/api/rvm/product), documented that `pageLength` supports a maximum of 50 items per page.

- **Other**  
  - Applied minor wording and formatting changes.

 

## 08-08-2025
* Added Endpoint to buffer coupon
* Added Endpoint to inform DRS Operator about burned Voucher
* Added Information about issued voucher in [POST - /transaction](https://kaucja.stoplight.io/docs/rvm-api/sbyw8qv8u7gj8-post-single-transaction) endpoint


## 0.9.2
* Removed RVM unblock Endpoints. Kaucja.pl will not require RVM locking for sealing

## 0.9.1

* Removed response from DRS [POST - /transaction](https://kaucja.stoplight.io/docs/rvm-api/sbyw8qv8u7gj8-post-single-transaction) endpoint. Only RVM Machine decides about receipt print.
* Added Optional field `lockingPersonId` in [POST - /bag-seal](https://kaucja.stoplight.io/docs/rvm-api/lqz2mv777xppe-call-performed-when-bag-seal-is-performed) and [POST - /bag-replacement](https://kaucja.stoplight.io/docs/rvm-api/3r55dg8tllqbx-trigger-an-replacement-action-for-rvm) endpoints to try corelate person with bag to prevent frauds
* Updated [POST - /machine/{id}](https://kaucja.stoplight.io/docs/rvm-api/bu3ambgd8l19t-machine-update) to support registering RVM Machine in Kaucja DRS by RVM Machine Vendor. Removed location field. Will be correlated with Collection Point
