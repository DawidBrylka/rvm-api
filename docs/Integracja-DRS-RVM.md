# RVM API Readiness Overview

**Requirements for RVM producers before API implementation**

This document defines the practical readiness areas that an RVM producer must address before connecting RVM software with the Kaucja.pl API. It focuses on hardware assumptions, machine-side software flow, offline behaviour, bag handling and negative scenarios.

**Audience:** RVM producers, RVM software teams, implementation teams, installers and technical reviewers  
**Scope:** Pre-implementation API readiness, machine-side software behaviour and the per-machine installation journey  
**Main focus:** Hardware assumptions, API capabilities, offline behaviour, bag replacement, seal handling and operational activation of each new RVM

## 1. Purpose

This overview gives RVM producers a clear starting point before API implementation begins. It explains what must be designed and verified before the producer connects RVM software with the Kaucja.pl API.

The document does not replace the endpoint reference but explains the operational meaning of the key API capabilities and the machine-side behaviour expected around them.

- **Hardware readiness:** the RVM must meet the minimum hardware requirements required for production approval.
- **Software flow readiness:** the producer must understand the expected flow for product data, transactions, vouchers, bags and seals.
- **Negative scenario readiness:** the producer must define how the machine behaves when something fails, especially around registration, offline work, seals and bag replacement.

## 2. Scope

In this document, API readiness means the producer’s preparation to connect RVM software with the Kaucja.pl API. It covers the design decisions, endpoint usage and machine behaviour that must be considered before implementation and testing.

The document also includes a separate operational installation journey. This journey is followed each time a new RVM is installed at a Collection Point after the producer has prepared the required API and machine-side behaviour.

> **Core distinction**  
> API readiness describes what the producer must build and verify before implementation and tests. The operational installation journey describes how each approved machine is activated at a specific Collection Point.

## 3. Operational installation journey for each new RVM

The steps below describe the operational path followed when a new RVM is installed at a Collection Point. This is separate from the producer’s API implementation work, but it depends on the same machine registration, product catalogue, transaction, bag and seal flows.

During this journey, the producer uses the relevant API endpoints for the installed machine: `POST /machine`, `POST /machine/{id}`, `HEAD /machine/{id}`, `GET /product`, `GET /product/{ean}`, `GET /voucher/buffer` where offline voucher handling is expected, `POST /transaction`, `POST /bag-replacement` and, only in the delayed-sealing model, `POST /bag-seal`.

1. **Create and activate the Collection Point.** The client creates the Collection Point in the Kaucja.pl portal and makes sure the point is active before the machine is configured.
2. **Provide the Collection Point number to the RVM producer.** The Collection Point number is used by the producer or installer to connect the machine to the correct point in DRS.
3. **Install the physical machine on site.** The producer or service team places the RVM in the agreed location and prepares the machine for configuration.
4. **Configure the machine for the correct Collection Point.** The producer sets the required technical data, including the Collection Point reference, machine model and bin or bag configuration where applicable.
5. **Register or update the machine in DRS.** The producer uses `POST /machine` to register the RVM or `POST /machine/{id}` to update it. The DRS machine identifier returned or confirmed during this step must be stored and used in further operational communication.
6. **Verify the registered machine.** The producer may verify the machine with `HEAD /machine/{id}`. The client can also verify that the created machine is visible under the correct Collection Point in the Kaucja.pl portal.
7. **Synchronize product data and voucher buffer, where applicable.** The machine uses `GET /product` and, when needed, `GET /product/{ean}` to retrieve product data. If offline voucher handling is expected, the machine should use `GET /voucher/buffer`. The synchronization with the product database should happen not less often than once every 24 hours.
8. **Confirm voucher or deposit payout handling.** The RVM does not redeem vouchers by itself. The Collection Point must have a defined payout path through an integrated POS system or the Kaucja.pl mobile application. This should be confirmed by the client.
9. **Confirm bag and seal handling.** The producer must confirm how the machine sends bag data through `POST /bag-replacement`. If the RVM has a seal scanner, the seal should be assigned during bag replacement / closure. `POST /bag-seal` is used only when delayed sealing is intentionally supported.
10. **Perform an on-site operational check.** Before handover, the producer should confirm that machine registration, product recognition, transaction handling, bag replacement and seal handling work as expected, including negative scenarios around rejected bag data and rejected or missing seals.
11. **Hand over the machine for operational use.** The machine should only be treated as ready for use when the correct Collection Point relation, machine identifier, voucher handling path and bag/seal process are confirmed.

> **Installation rule**  
> A correctly implemented API flow does not remove the need for per-machine activation checks. Every new RVM must be connected to the correct Collection Point and must have a clear voucher payout and seal-handling path before operational use.

> **Primary operational priority**  
> Correct bag data sent to Kaucja.pl through `POST /bag-replacement` is the highest-priority part of the RVM flow. The producer must design and test negative scenarios for rejected bag replacement, missing or invalid seals, duplicate seals, connectivity loss and safe retry logic.

## 4. Primary API scope

The primary API scope focuses on the capabilities that matter most for a working RVM flow. Not every endpoint exposed in the API reference is an absolute requirement for every producer or every setup.

Within this scope, the most important priority is correct bag data. Kaucja.pl settles based on bag contents, so `POST /bag-replacement` must be implemented with particular attention to complete data, negative scenarios and safe recovery after failed calls.

| Area | Endpoint | Priority | Operational meaning |
| --- | --- | --- | --- |
| Machine registration / update | `POST /machine`<br>`POST /machine/{id}`<br>`HEAD /machine/{id}` | Required | Registers or updates the RVM and verifies that the machine is known in DRS. The DRS machine identifier must be stored and used consistently in later calls. |
| Product catalogue | `GET /product`<br>`GET /product/{ean}` | Required | Provides product data used by the RVM to verify deposit-bearing packaging before transaction data is sent to Kaucja.pl. At this stage Kaucja.pl does not accept transactions containing non-DRS packaging, so such packaging must be identified and handled before the transaction request is sent. The synchronization with the product database should happen not less often than once every 24 hours. |
| Transaction reporting | `POST /transaction`<br>`POST /transaction/bulk`, if agreed | Required | Sends return transaction data to DRS and receives payment or blocking information. The bulk variant is used only if agreed in the implementation flow. |
| Voucher buffer | `GET /voucher/buffer` | Recommended | Advisable for offline-capable machines. It allows the RVM to operate in a controlled way when online voucher generation is temporarily unavailable. |
| Bag replacement | `POST /bag-replacement` | Critical | Reports that a bag has been replaced or closed and sends the bag content. This is the highest-priority call because Kaucja.pl uses bag contents for settlement and most operational issues originate here. |
| Bag seal | `POST /bag-seal` | Optional | Used only if the producer intentionally allows clients to close bags first and seal them later. This is not the recommended default because it increases the risk of human error. |

## 5. Recommended machine-side flow

The RVM software must support a consistent operational flow. The exact implementation may differ between producers, but the system must preserve reliable relations between the machine, product catalogue, transactions, vouchers, bags and seals.

1. Register or update the machine and store the correct DRS machine identifier.
2. Verify that the machine is known in DRS before sending production transaction or bag data.
3. Synchronize the product catalogue from DRS and keep the latest successful version locally.
4. Validate returned packaging against the available catalogue logic before transaction data is sent to Kaucja.pl.
5. Send transaction data to DRS, or queue it safely if the agreed offline model allows this.
6. Use voucher buffer logic where offline work is expected or required.
7. Track bag contents during the machine’s operation.
8. Send complete bag replacement data to Kaucja.pl when the bag is replaced or closed.
9. Assign and validate the seal during the bag replacement / closure flow whenever possible.
10. Use delayed seal reporting only as an intentional exception with clear controls.
11. Retry failed calls safely without creating duplicate transactions, duplicate bags or inconsistent seal assignments.

## 6. Bag replacement, seals and settlements

Bag replacement and seal handling must be treated as settlement-relevant parts of the flow.

Kaucja.pl performs settlements based on bag contents. The bag replacement request is therefore the key moment where the RVM reports what is inside the bag and makes this data usable for later operational and settlement processes. This part of the flow must be treated as the highest implementation priority.

> **Important settlement rule**  
> A bag without a valid seal in DRS is not included in settlements. The machine-side flow must therefore prevent missing seals wherever possible.

- Bag contents must be reported reliably when the bag is replaced or closed.
- The seal is assigned during the bag replacement / closure flow whenever the machine supports this.
- A bag must not be treated as successfully completed if the seal was rejected or not assigned.
- If DRS rejects the bag replacement request, the bag must remain in a recoverable state.
- The operator or service team must be able to see that the bag was not accepted by DRS.
- Delayed sealing is avoided by default and allowed only when the producer intentionally supports it.

## 7. Delayed sealing and the bag-seal endpoint

The `POST /bag-seal` endpoint is optional. It is relevant only if the producer wants to allow a flow where a client closes a bag and applies or reports the seal later.

This flow is not recommended as the default approach. It creates additional human-error risk because a bag may physically exist and contain packaging, while the seal is missing in DRS.

The recommended approach is to require the seal during bag replacement / closure and to block or clearly mark the process until the seal is valid and accepted.

- Delayed sealing must be visible to the operator or service team.
- The machine must clearly distinguish between a closed-and-sealed bag and a bag waiting for a seal.
- The machine must not silently treat an unsealed bag as settlement-ready.
- The producer must describe how a missing seal is corrected and how the correction is sent to DRS.

## 8. Offline operation

The RVM must have defined behaviour for temporary connectivity loss.

- The machine stores the latest successfully synchronized product catalogue locally.
- The machine uses voucher buffer logic where it is expected to continue work without live voucher generation.
- The producer must define whether the RVM keeps accepting packaging when the connection is lost.
- The machine must avoid accepting non-deposit packaging only because live validation is unavailable.
- Queued transactions and bag operations must be retried safely after connection is restored.
- Retry logic must avoid duplicate transactions, duplicate bag reports and inconsistent seal assignment.

## 9. Negative scenarios

Before API tests begin, the producer must be able to describe the machine behaviour in the scenarios below. Negative scenarios around bag replacement, seals and retries are especially important because they directly affect whether bag data can be used for settlement.

The description covers what the operator sees, what state the machine keeps internally and how the process can continue or recover.

| Scenario | What can happen | Expected behaviour |
| --- | --- | --- |
| Duplicate seal number | The operator uses a seal number that has already been registered in DRS. | The machine clearly rejects the seal, allows a new unique seal to be provided, and keeps the rejected seal separate from the accepted one. |
| Invalid seal number | The seal is too short, too long, has the wrong prefix or fails checksum validation. | The machine detects the issue as early as possible and does not close the bag successfully with that seal. |
| No internet during transaction | The machine loses connectivity during a return session. | The machine follows a defined offline model, uses the latest available catalogue, uses voucher buffer if applicable, and safely submits or retries data after reconnection. |
| Voucher buffer unavailable or exhausted | The machine cannot obtain or use buffered vouchers during offline work. | The machine clearly defines whether it stops, blocks payout, or continues without issuing a voucher. The user and operator must not be misled. |
| Failed machine registration | The machine cannot be registered or verified in DRS. | The machine does not behave as if registration succeeded. Registration can be repeated after configuration or Collection Point data is corrected. |
| Wrong machine identifier | The machine sends data with an incorrect or outdated machine ID. | Rejected operations remain recoverable. After correction, the machine does not duplicate transactions, bags or seals. |
| Rejected transaction | DRS rejects a transaction request. | The machine keeps the transaction in a clear state and applies retry or cancellation rules without creating duplicates. |
| Rejected bag replacement | DRS rejects the bag replacement request. | The bag remains recoverable and is not treated as settlement-ready until the issue is corrected. |
| Missing seal after bag closure | A bag has reported contents but no valid seal in DRS. | This state should be prevented by default. If delayed sealing is allowed, the machine must force visibility and recovery of the missing seal. |

## 10. Hardware requirements

Hardware requirements are a separate part of RVM readiness. The hardware requirements document is a controlled attachment to this documentation page.

Each RVM must meet the required hardware conditions before it can be approved for production use. A working API connection alone is not sufficient if the machine does not meet the required hardware and operational conditions.

- packaging recognition capabilities,
- network connectivity and defined offline behaviour,
- local storage for product catalogue data, voucher buffer and retry queues where applicable,
- bag content tracking,
- seal scanning or another controlled seal assignment method,
- operator messages for rejected or incomplete actions,
- diagnostic logs for transactions, bags, seals and failed API communication.

The full set of requirements is to be found here: **[LINK]**

## 11. Production approval principle

Production approval must be based on both API correctness and the operational behaviour of the physical machine. The RVM must be able to send correct data, handle failures safely and maintain settlement-relevant information about transactions, bag contents and seals.

A machine that can send successful API requests but cannot handle bag replacement, seals, offline work or recovery scenarios correctly is not ready for production.

> **IMPORTANT:** Before proceeding with integration testing with Kaucja.pl, the RVM producer has to have acquired the accreditation from the inter-operator group.
