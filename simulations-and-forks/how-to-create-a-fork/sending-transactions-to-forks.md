---
description: >-
  Besides manually running Simulations in the Dashboard, you can use the
  JSON-RPC to  integrate Hardhat and/or Ethers (or any other Ethereum access
  library) and simulate transactions you send from code.
---

# Sending Transactions to Forks

## Working with Hardhat

Tenderly Forks expose a Fork URL you can use to define a **tenderly** network in the Hardhat user config. When running Hardhat scripts and tasks, by specifying `--network tenderly`, you'll forward all transactions sent through Ethers to a Fork for a Simulation.

Here's how to configure Hardhat to use a Tenderly Fork as a network:

```diff
// File: hardhat.config.ts
// -- snip --
const config: HardhatUserConfig = {
  solidity: "0.8.9",
  networks: {
    // -- snip --
+   tenderly: {
+     chainId: 3, // chain you forked
+     url: "FORK-URL",
+   },
  }
  // -- snip --
}
export default config
```

The placeholder `FORK-URL` is the JSON-RPC URL exposed by the Fork. It uses the following format:`https://rpc.tenderly.co/fork/{FORK_ID}`. You can [find the JSON-RPC URL](how-to-get-a-fork-json-rpc-url-and-id.md) in the Dashboard.

Your existing hardhat scripts should work as expected.

```javascript
import { ethers } from "hardhat";

async function main() {
    // show accounts created on the fork
    console.log(await ethers.provider.listAccounts());
    // ... etc
}
```

## Working with Ethers

If you want to connect to a Tenderly Fork using Ethers, it's sufficient to create a `JsonRpcProvider` instance:

```java
const provider = new JsonRpcProvider(FORK_URL);
const myContract = new ethers.Contract(ADDRESS, ABI, provider.getSigner());

// 10 accounts with balance 100 available
const accounts = await provider.listAccounts();
const signers = accounts.map(acc => provider.getSigner(acc));

```

Any transactions sent to `myContract` will be directed to and simulated on a Fork.

## Controlling the Fork

To be fully in control of all the aspects of Simulations happening on a Fork, such as account balances, mined blocks, or even passage of time, explore the Fork Customization via API.

{% content-ref url="../simulation-api/tenderly-fork-customization-via-api/" %}
[tenderly-fork-customization-via-api](../simulation-api/tenderly-fork-customization-via-api/)
{% endcontent-ref %}
