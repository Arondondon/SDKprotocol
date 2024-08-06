# SNET SDK protocol

SDK protocol describes the structure, entities and their functionality for the SDK 
regardless of the implementation language.

## Contents

- [SDK structure](#structure)
- [Entities description](#entities)
- [User story]()

## Structure

![Entities diagram](resources/ClassDiagram_v4.png)

All entities can be divided into 4 layers:
1. `Config Layer` contains entities with many fields, that are needed to configure SDK.
2. At `Interface Layer` there are entities through which users have access to `Functional Layer`.
3. `Functional Layer` contains in its entities all the functionality of the SDK that users may need.
4. `Tools Layer` contains entities with various auxiliary functionality 
required for the operation of the functional level.

> There are 4 types of relationships in the diagram:\
> ![relationships](resources/connections.png)

## Entities

- [Config Layer](#config-layer)
  1. [Account](#account)
  2. [EthAccount](#ethaccount)
  3. [Config](#config)
- [Interface Layer](#interface-layer)
  1. [SNETEngine](#snetengine)
  2. [Service](#service)
  3. [ServiceByGRPC](#servicebygrpc)
- [Functional Layer](#functional-layer)
  1. [Contract](#contract)
  2. [EthContract]()
  3. [MPEContract]()
  4. [RegistryContract]()
  5. [MetadataProvider]()
  6. [IPFSMetadataProvider]()
  7. [PaymentChannel]()
  8. [Channels]()
- [Tools Layer]()
  1. [GRPCLibGenerator]()
  2. [AGIXContract]()
  3. [ServiceMetadata]()
  4. [OrgMetadata]()
  5. [IPFSUtils]()
  6. [TransactionHandler]()
  7. [LedgerTransactionHandler]()
  8. [KeyOrMnemonicTransactionHandler]()
  9. [PaymentStrategy]()
  10. [DefaultPaymentStrategy]()
  11. [FreeCallPaymentStrategy]()
  12. [PrepaidPaymentStrategy]()
  13. [PaidCallPaymentStrategy]()

---

### Config Layer

#### Account

`Account` is an abstract entity. It contains abstract functions, such as `get_name`
and `get_nonce`. It's the base class to `EthAccount` and others that are planned to be implemented to work 
with other blockchains on the SNET platform.

---

#### EthAccount

`EthAccount` implements `Account`. It is needed to identify user wallet in the Ethereum blockchain.
It contains all the fields, that need to be known for it to work correctly 
(e.g. private key, mnemonic phrase, wallet address etc.). 
Its instance is created using a name, account type (key, mnemonic or ledger) 
and corresponding secret, the remaining fields are initialized automatically by Web3 by default. 
Contains almost no logic, only account data.

##### data

- name
- address
- account type
- private key (optional)
- mnemonic phrase (optional)
- index (optional)
- nonce

##### functionality

- address determination by key/mnemonic/ledger by using Web3
- updating the nonce value and comparison it with the value received via Web3

---

#### Config

`Config` is the main configuration entity containing all the data needed to work 
in the SNET platform. It contains entry point, network name, blockchain type, smart contract 
addresses (optional) and some other options. These values are specified when creating 
an instance, which is then passed to `SNETEngine`.

##### data

- blockchain entry point
- blockchain type
- network name
- accounts (list/map)
- MPE, Registry, AGIX contracts addresses (optional)
- forced update protobuf files (bool) (optional)
- concurrency (bool) (optional)

##### functionality

- adding an account 
- getting an account

---

### Interface Layer

#### SNETEngine

`SNETEngine` is the most important entity from the user side. `Config`'s instance 
is installed in it, and new accounts and services are created by using it. 
`SNETEngine` creates and contains instances of almost all entities from 
`Functional Layer` (`MPEContract`, `RegistryContract`, `MetadatProvider` etc.), 
so they are accessed through it, and therefore most of the SDK functions are accessible 
from `SNETEngine`.

##### data

- `Config`'s instance
- an instance of a Web3 library entity that will be passed to all dependent entities
- `MPEContract`'s instance
- `RegistryContract`'s instance
- `AGIXContract`'s instance
- an instance of the metadata provider implementation
- `GRPCLibGenerator`'s instance
- `Channels`'s instance

##### functionality

- creating new `Account`
- creating and initializing new `Service`
- providing the user with access to the contracts and metadata provider or to their functionality

---

#### Service

`Service` contains the most important abstract method `call` for calling a service 
with given parameters. It's the base class to `ServiceByGRPC` and others that are 
planned to be implemented to access `daemons` using other protocols, such as REST API.

##### functionality

- call a service with given parameters (abstract)

---

#### ServiceByGRPC

`ServiceByGRPC` extends `Service`. It allows you to call service functions by 
communicating with the `daemon` via gRPC

##### data

-

##### functionality

- 

---

### Functional Layer

#### Contract

`Contract` is an abstract entity. It contains abstract methods to call _read_ and 
_write_ functions of smart contracts. It's the base class to `EthContract` and others that are planned 
to be implemented to work with other blockchains on the SNET platform.

---

#### EthContract

`EthContract` implements `Contract`. It allows you to call _read_ functions and perform
transactions for calling _write_ functions of smart contracts in Ethereum.

##### data

- 

##### functionality

- 

---
