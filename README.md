## Local Rococo Coretime Chain

This repository aims to provide a Zombienet configuration + instructions on how to create a local Coretime chain for testing.

### Prerequisites

- `polkadot` and `polkadot-parachain` binaries, which you can install with `cargo` or use the binaries on the GitHub release page
- [Zombienet](https://github.com/paritytech/zombienet) (can also be run with `npx`)
- `node` / `npm` / `npx` if you're using Zombienet this way

### Starting the chain

Once this repository is cloned and Zombienet is installed, run the following:

```bash
zombienet --provider native spawn ./coretime.toml
# or
npx --yes @zombienet/cli --provider native spawn ./coretime.toml
```

Within a couple of minutes, both the Coretime and relay chain should be producing blocks.

### Setting the broker configuration

You will notice that there are no cores available. We need to set a configuration for the broker pallet as follows (we cannot start a bulk coretime sale with an uninitialized broker):

1. Navigate to the Coretime chain in Polkadot.js: [Coretime Chain Extrinsics](https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9911#/extrinsics)
2. Use the `sudo > call` extrinsic
3. Select the `broker > configure` extrinsic, and put the following configuration:
  - advanceNotice: 10
  - interludeLength: 50,400
  - leadinLength: 100,800
  - regionLength: 5,040
  - idealBulkProportion: 100.00%
  - limitCoresOffered: null
  - renewalBump: 3.00%
  - contributionTimeout: 5,040

For details on what these fields mean, see the [Polkadot SDK Docs.](https://paritytech.github.io/polkadot-sdk/master/pallet_broker/type.ConfigRecordOf.html#fields)

After that, send the extrinsic with `Alice`.

### Starting the Coretime Sale

Once the configuration is set, you need to call the `broker.startSales` function. This function is responsible for starting the actual sales rotation, and will allow you to buy cores.

1. Navigate to the Coretime chain in Polkadot.js: [Coretime Chain Extrinsics](https://polkadot.js.org/apps/?rpc=ws://127.0.0.1:9911#/extrinsics)
2. Use the `sudo > call` extrinsic
3. Select the `broker > startSales` extrinsic, set extraCores to the amount of cores to start the sale with, and endPrice to 0.

Send the extrinsic with `Alice`, and the Bulk Coretime sale should begin. You should see UMP/DMP messages in the explorer once this starts







