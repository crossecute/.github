# crossecute

Run upgrades and other admin tasks across chains from one wallet, Safe, or DAO. Your team reviews and authorizes changes in one place.

```mermaid
flowchart LR
    SDK["SDK prepares"] --> Attest["Forge Attest verifies"]
    Attest --> Safe["Safe approves"] --> Protocol["Protocol executes across chains"]
```

- [Protocol](https://github.com/crossecute/protocol) sends instructions from your home chain and runs them on the destination chains.
- [SDK](https://github.com/crossecute/sdk) simulates the changes and prepares one Safe transaction for review.
- [Forge Attest](https://github.com/crossecute/forge-attest) reruns your script and checks that its output matches the transaction queued in Safe.

## Multichain multisig and governance comparison

✅ Yes · ❌ No in the documented workflow

| Solution | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| crossecute | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Chainlink MCMS | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| LayerZero OneSig | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Candide | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Safe + Zodiac Bridge | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ |
| Safe + Hyperlane ICA | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| Standard Safe | ✅ | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ |
| Wormhole MultiGov | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| Aave a.DI | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ |

1. Execute on multiple chains
2. One signing round for a multichain batch
3. Signatures are verified on the destination
4. One multisig deployed on a home chain only
5. Rotate signers on one chain to revoke their authority across chains
6. Nested multisig contracts as signers
7. Passkey signers
8. Supports token-voted proposals
9. Automatic destination execution from a home-chain dispatch
10. Cryptographically verifiable proposal scripts
11. Use the standard Safe app and hosted signature queue

Capability 3 refers to the original approvers' signatures. Capability 10 counts tooling supplied by each project. Forge Attest also works with compatible Safe transactions from other workflows.

Capability 5 covers authorization of new operations. For crossecute, the check applies to plain mode. Safe-signed receivers must apply the authority update. For Zodiac Bridge and Hyperlane, the check applies to calls authorized by the home Safe. A destination Zodiac Safe's independent owners retain their local authority. MultiGov and Aave Governance use token votes. [crossecute authority updates](https://github.com/crossecute/protocol/blob/main/docs/safe/safe-authority-updates.md)

- **crossecute** works with an existing Safe using standard Safe transactions and Safe's hosted queue. In plain mode the home Safe can verify passkey signatures. With a compatible guard, Safe-signed mode verifies signatures from wallet keys and nested Safes on the destination. A DAO governor or its timelock can own the conduit and dispatch operations approved by token holders. Automatic execution depends on the delivery route. [SDK guide](https://github.com/crossecute/sdk/blob/main/docs/usage.md) · [Safe queue](https://docs.safe.global/core-api/safe-queue-service-reference) · [Safe passkeys](https://docs.safe.global/sdk/signers/passkeys) · [Safe-signed dispatch](https://github.com/crossecute/protocol/blob/main/docs/safe/safe-signed-dispatch.md) · [Message flow](https://github.com/crossecute/protocol/blob/main/docs/architecture/message-flow.md)
- **Chainlink MCMS** supports nested threshold groups of individual signers. Teams coordinate signing themselves and can add signatures to a proposal file with the SDK. Operators submit execution calls on the target chains. [Signer groups](https://github.com/smartcontractkit/ccip-owner-contracts#manychainmultisig-considerations) · [Signing](https://smartcontractkit.github.io/mcms/usage/signing-proposals/) · [Execution](https://smartcontractkit.github.io/mcms/usage/executing-proposals/)
- **LayerZero OneSig** uses a separate signature collection server in its reference workflow and also documents how to combine signatures locally. An executor submits the signed operation on each target chain. [OneSig protocol](https://github.com/LayerZero-Labs/onesig/blob/main/PROTOCOL.md)
- **Candide Safe Unified Account** uses AbstractionKit to sign multichain UserOperations. Each destination Safe verifies signatures, including WebAuthn passkeys and other contract signatures. Submission can use a hosted bundler or be done directly. [Passkey signing](https://docs.candide.dev/wallet/plugins/passkeys/) · [Safe Unified Account architecture](https://github.com/candidelabs/safe-4337-multi-chain-signature-module/blob/main/ARCHITECTURE.md) · [Submission options](https://docs.candide.dev/wallet/guides/walkaway-guide/#bundler)
- **Safe + Zodiac Bridge** lets a home Safe or DAO governor control a destination Safe through its bridge module. The documented bridge flow includes a destination submission step. [Bridge module](https://github.com/zodiaceco/zodiac-module-bridge#flow)
- **Safe + Hyperlane Interchain Accounts (ICA)** lets a home Safe or DAO governor send calls to a remote account. The destination authenticates the Hyperlane message. [Accounts API](https://github.com/hyperlane-xyz/hyperlane-quickstart#accounts-api) · [Router contract](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/main/solidity/contracts/middleware/InterchainAccountRouter.sol)
- **Standard Safe** supports nested Safes, passkey owners, and a hosted signature queue. Each chain has its own Safe deployment and approvals, even when the address is shared across chains. [Safe accounts](https://docs.safe.global/advanced/smart-account-overview) · [Multichain deployment](https://docs.safe.global/advanced/smart-account-multi-chain-deployment)
- **Wormhole MultiGov** tallies token votes from multiple chains on a hub and relays approved operations to their destinations. It authenticates dispatches through Wormhole messages. [Proposal flow](https://wormhole.com/docs/products/multigov/concepts/proposal-flow/)
- **Aave a.DI (Aave Delivery Infrastructure)** delivers token-approved Aave Governance proposals across chains. The destination a.DI controller confirms messages using a configurable threshold of approved bridge adapters. The governance payload queues on arrival and requires a separate execution call after the destination timelock. [a.DI architecture](https://github.com/aave-dao/aave-delivery-infrastructure/blob/main/docs/overview.md) · [Aave proposal lifecycle](https://aave.com/docs/ecosystem/governance#onchain-proposal-lifecycle)
