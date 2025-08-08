---
stoplight-id: qlcl5lzd2yxfb
---

# CHANGELOG

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
