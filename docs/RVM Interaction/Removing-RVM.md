---
stoplight-id: w2tx8sc7ikulc
internal: true
---

![RVMDeletion.png](../../assets/images/RVMRemoval.png)

# Machine Deletion Process

## Overview
The **Machine Deletion** process is performed by **DRS** through their mobile or web application. It ensures that a machine is properly removed from **RVM Cloud** when it is no longer in service or when a machine replacement occurs at a Collection Point.

## Process Flow
1. **Start:** The deletion process is initiated in the **DRS system** when a machine is identified for removal.
2. **API Endpoint:** The request is made to the `/machine/{id}/delete` endpoint in **RVM Cloud**.
3. **Response:** RVM Cloud processes the deletion request and returns a **confirmation**.

<!--
type: tab
title: RVM
-->

Representation of API endpoints exposed by **RVM Cloud** to complete this process.

### DELETE /machine/{id}/delete

For a full overview of this endpoint, please visit: [DELETE - /machine/{id}/delete]

<!-- type: tab-end -->