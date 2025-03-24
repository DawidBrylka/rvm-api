Spis treści

[API between Kaucja.pl and RVM 1](#_Toc192864046)

[API list 1](#_Toc192864047)

[1\. Data exchange from Deposit Product Catalogue (Katalog Produktów Kaucyjnych) 1](#_Toc192864048)

[1.1. New Product added 1](#_Toc192864049)

[1.2. Deposit Product Catalogue sync 1](#_Toc192864050)

[2\. RVM status healthcheck 1](#_Toc192864051)

[3\. RVM removal 1](#_Toc192864052)

[4\. New RVM registration 1](#_Toc192864053)

[5\. Transaction data exchange 1](#_Toc192864054)

[5.1. Transaction data transfer to DRS 1](#_Toc192864055)

[5.2. Lost data sync 1](#_Toc192864056)

[6\. Bag replacement 1](#_Toc192864057)

[6.1. RVM with scanner 1](#_Toc192864058)

[6.2. RVM without scanner 1](#_Toc192864059)

# Document description

This document is describing API expected by Kaucja.pl for contact between RVM and Kaucja.pl core DRS system.

# API list

## Data exchange with Deposit Product Catalogue (Katalog Produktów Kaucyjnych)

### New Product added

DRS

• [POST - /product](rvm-openapi.yaml/paths/~1machine/post)

RVM

• [POST - /product](https://kaucja.stoplight.io/docs/rvm-api/a4c7ecf770c6d-add-or-update-product)

### Deposit Product Catalogue sync

**DRS**

• [GET - /product](DRS.html#api-Products-productGet)

## RVM status healthcheck

**RVM**

• [GET - /machine](https://kaucja.stoplight.io/docs/rvm-api/7a1d5570a1001-get-all-machine-data)

## RVM removal

**RVM**

[DELETE - /machine/{id}/delete](https://kaucja.stoplight.io/docs/rvm-api/3b55145e14cda-delete-rvm)

## New RVM registration

**RVM**

[POST - /machine](https://kaucja.stoplight.io/docs/rvm-api/807f60ee83b19-add-new-rvm)

## Transaction data exchange

### Transaction data transfer to DRS

**DRS**

[POST /transaction](DRS.html#api-Transaction-transactionPost)

### Lost data sync

**DRS**

[**POST - /transaction**](DRS.html#api-Transaction-transactionBulkPost)

**RVM**

[**GET - /transaction**](https://kaucja.stoplight.io/docs/rvm-api/9ff0ca4784379-get-transaction)

[**PUT - /transaction/{}/confirmation**](https://kaucja.stoplight.io/docs/rvm-api/4f2a8c5f717a3-get-transaction)

## Bag replacement

### RVM with scanner

The input to the process can be both information from the RVM that the bag is full and an exchange resulting from the initiative of a Collection Point employee.

For an RVM that has an external scanner, the process of scanning the seal and transmitting information about it to the DRS Operator is done using it.

### RVM without scanner

For RVMs that do not have an external scanner, the process of scanning the seal is done via a mobile application provided by the DRS Operator, or an equivalent device.

It is important to understand that in order to keep track of all bags only after receiving confirmation of sealing from mobile app RVM should be unblocked.

DRS

[POST - bag-is-full](DRS.html#api-Rvm-bagIsFullPost)

[POST - bag-replacement](DRS.html#api-Rvm-bagReplacementPost)

[POST - bag-seal (between PZ - DRS)](DRS.html#api-Rvm-bagSealPost)

RVM

[PUT - /machine/{id}/unblock](RVM.html#api-Machine-machineIdUnblockPut)