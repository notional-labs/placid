# placid

mitigations to asa-2023-002, which is part of a larger set of issues reported to:

* Interchain Foundation
* Informal Systems
* Amulet
* Strangelove Labs

...and were inaccurately distilled into asa-2023-002 by the above teams.

a testing tool called [hardhat](github.com/somatic-labs/hardhat) has been built and chain teams can use this tooling to test the performance of their chain under load.

## Notes

The [security bulletin](https://forum.cosmos.network/t/amulet-security-advisory-for-cometbft-asa-2023-002/11604) was released against my express wishes by [Amulet](https://twitter.com/amuletdotdev), the security contractor to [Interchain Foundation](https://interchain.io) wifh approval by [Informal Systems](https://informal.systems).

The issue disclosed in Amulet's post affects every chain using CometBFT and is aggrevated by a few other outstanding issues in the cosmos stack.  When they said "degraded" and said "affect consensus participation" they should have said "can result in halts and/or 30 minute block times also maybe some OOM issues and immense griefing for validators and node operators that will result in rpc downtime." 

* RPC downtime was observed on all tested chains
* Umee had 10% of vote power go down and atay down despite the transactions stopping.
* Mars protocol did not charge for gas and had 22mb blocks, which allowed hardhat users to
  * extend block times 10x their normal duration
  * disrupt ibc
  * store gigabytes for free, treating the chain like a free hard disk

Chain teams have been given materially inaccurate information by the foundation and getting this issue addressed has required testing on mainnets and months of work.  

It has been necessary to vet these findings with Seal911, az it has been proven impossible to report and discuss these issues with Interchain Foundation funded teams. 

There is not currently any one-size-fits-all solution.

The issue disclosed in Amulet's post is not yet fixed, but there are ways to mitigate it, perhaps fully.

The security bulletin from [Amulet](https://twitter.com/amuletdotdev) was posted against the direct and clearly expressed wishes of the Notional team.

[asa-2023-002](https://forum.cosmos.network/t/amulet-security-advisory-for-cometbft-asa-2023-002/11604) is badly miscategorized, and its severity goes as high as critical, depending on what the chain in question actually does.

## Plan

* assist validators and chain teams with the implementation of mitigations
* Work on issues with the mempool and block gossip
* release solid fixes
* address procedural issues that caused the interchain foundation to bungle these items since 2021

## Mitigations

It is important to note that the mitigations will vary somewhat from chain to chain. 

### Have a block size of less than 2mb unless you need larger

[2mb blocks](https://forum.cosmos.network/t/increase-maxblocksize-from-200k-to-2mb), not smaller, and be mindful of contract upload sizes for your chain, which may require larger blocks.  Don’t set block size to more than 5mb, especially if your application is time sensitive, for example, handling liquidations on ibc counterparties. 

### Validators should peer directly with one another

* Don't peer directly with validators you don't trust
* Every validator should make these decisions for themeselves

* Methods - Configuration
  * Add other validators node IDs to your unconditional peers
  * add other validators ID@address to your unconditional peers

* Methods - VPN techniques allowing you to make a direct connection or even form a private fastest path routing network

  * Zerotier
  * TailScale
  * WireGuard

### 100 gas per byte

Raising the price of bytes will ensure that exploiting

* [Cosmos Hub Example](https://forum.cosmos.network/t/last-call-increase-the-size-of-the-memo-field-to-100kb-and-10x-the-cost-of-bytes/11500/11)

### Validators should not perform indexing

Banana king transactions break indexing up and down the stack and **may** be responsible for cases where validators experience oom issues. 

### Jail validators faster

If validators are jailed more rapidly, there is less opportunity to achieve a full halt, and it is more expensive in terms of cost per unit time.

* [adjusting signed blocks window downwards](https://forum.cosmos.network/t/adjust-min-signed-per-window-to-80/11808/1) 
* min_signed_blocks_per_window upwards

### lower downtime slash

With a smaller downtime window or hgiher min_signed_blocks_per_window, validators are more likely to be jailed.  We should not bother to slash them if this occurs.

* [Cosmos Hub Example](https://forum.cosmos.network/t/eliminate-the-downtime-slash-and-reduce-downtime-jail/11783/10)

### decrease number of transactions the mempool can store

Default:
```toml
# Maximum number of transactions in the mempool
size = 5000
```

Suggested: A few blocks of peak traffic
```toml
# Maximum number of transactions in the mempool
size = 1000
```

* from 5000 to a reasonable value i.e. 1000 or less for now
* if you need more for some reason, do please note that normal transactions are really, really small compared to abuse transactions (1/100th or less in most cases)... so, make sure to reduce mempool size **in bytes** as that is more impactful in resolving these issues  
* the mempool doesn't really need the ability to store more than a few blocks worth of transactions at peak traffic.

### reduce the size of your chains mempool in bytes

Default:
```toml
max_txs_bytes = 1073741824
```

Suggested: block size * 10

This exadmple uses 2mb blocks, which were tested on the cosmos hub replicated security testnet, and found to be a good value. 
```
max_txs_bytes = 20000000
```

* Tested on Cosmos hub replicated security testnet with help from CryptoCrew and Hypha
* Should make the issue less impactful as a smaller mempool will not trigger issues wifh recheck

### Beware of Banana King transactions and upgrade to ibc v8 asap

* [Banana King's Coming Out Party](https://x.com/web3_analyst/status/1635687287962112000?s=20)
* [Banana King's Fix](https://github.com/cosmos/ibc-go/issues/4859)

### do not use x/globalfee to for gas bypasses

* gas bypasses are dangerous to smooth operation of a blockchain and should not be used.

### discontinue usage of x/globalfee

* no comment

### Validators should enforce a globally consistent gas price using the configuration options for that

* use governance proposals to globally set a gas price, even if that cannot be enforced by x/globalfee, so that validators have a clear picture of what they should set gas prices to, instead of keeping that information unclear as it often is
* On the cosmos hub today, at minimum, Binance is not enforcing gas prices.  Tbruelle on the forum has more information. 

### Validators should ensure minimum 1gbps bandwidth and operate from bare metal nvme devices

* In testing on Celestia it was clear that validators with better hardware performed better in testing.
* A survey of hardware was performed on Celestia
* Injective has been recommending 5gbps internet connections, and this seems a prudent recommendation for validators. 

## Issues With Process

This repository, and 8 weeks of continuous work by numerous ecosystem participants, none of whom are compensated by the foundation, was made necessary by years of proceduarl issues that stem from the opaque way of working and culture at [Interchain Foundation](https://interchain.io).  Interchain Foundation funded teams were directly unhelpful, and ultimately directly hostile to this work.  In retribution for continuing this work, my entire company has been removed from the #cosmos-tech slack channel.   

* Numerous emails to security@interchain.io recieved no reply
* Informal seems to control the disclosure process for comdt issues.... I think.  But as a reporter it seemed to me that the only coordinated thing was hostility.
* Scripts provided for reproduction were not actually run -- if they had been, these issues would have been seen very clearly
* Issue was released against the recommendations of the reporter
* Reporter asked for calls with Amulet and Informal, recieved no response
* Report contained inaccurate information about issue

While I cannot recommend reporting via security@interchain.io or HackerOne currently, due to the foundation's broken processes and non-responsiveness, I do recommend that anyone with security issues to report, report thusly:

* Any issue in Cosmos: Osmosis
* Comet: Celestia
* CosmosSDK: Binary Builders
* IBC: Interchain GMBH
* x/wasm: Confio

I cannot in good conscience recommend reportin anything via amulet.

## Funding

for cosmos hub related portions of this issue has been provided by the cosmos hub community pool via Proposal 104, and the AADAO grant to CryptoCrew.  No meaningful aid was provided by the Interchain Foundation, Amulet, or Informal Systems.

This work has been entirely unfunded since the termination of cosmos hub proposal 104. 

The issues mitigated here have been reported by Notional and others to the Interchain Foundation since 2021.  This issue occured recently on Stride and that re-started the investigation into its causes.  
