# Vitreus Parachain Template

This repository is a fork of [Polkadot SDK's Parachain Template](https://github.com/paritytech/polkadot-sdk-parachain-template) adapted for the **Vitreus ecosystem**.

The template provides a minimal starting point for building a Substrate-based parachain with custom runtime logic and XCM configuration.

---

## Overview

This fork introduces several modifications compared to the upstream template:

* Integration with the **Vitreus relay chain**
* Runtime and chain spec updated to use the **VNRG** token with **18 decimals**
* Support for teleporting relay chain assets into the parachain
* Free execution for specific relay-chain XCM messages

---

## Token Configuration

The parachain runtime uses **VNRG** as the native token.

Token parameters:

* **Symbol:** `VNRG`
* **Decimals:** `18`

The chain specification and runtime constants were updated accordingly to reflect the new token configuration.

---

## XCM Configuration

This parachain supports teleporting the following relay-chain assets:

* **VNRG**
* **SNRG**
* **LNRG**

When received via teleport, these assets increase the recipient's native balance.

Teleport behavior:

* Teleports are accepted only from the relay chain
* Execution of teleport messages is unpaid

Typical teleport message structure:

```
ReceiveTeleportedAsset
ClearOrigin
BuyExecution
DepositAsset
```

The XCM barrier ensures that only this exact instruction pattern is accepted for unpaid execution.

---

## Building

Build the project:

```
cargo build --profile production --workspace
```

### Build Issues

#### RocksDB compilation error on newer compilers

When building with newer GCC/Clang versions, the bundled RocksDB dependency may fail to compile due to a missing `cstdint` include.

If you encounter this issue, build the project with:

```
CXXFLAGS="-include cstdint" cargo build --profile production --workspace
```

See the upstream issue for more details:  
https://github.com/facebook/rocksdb/issues/13365
