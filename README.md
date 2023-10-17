# placid
mitigations to https://forum.cosmos.network/t/amulet-security-advisory-for-cometbft-asa-2023-002/11604, which was released against my express wishes by [Amulet](https://twitter.com/amuletdotdev), the security contractor to [Interchain Foundation](https://interchain.io).


## Notes

https://forum.cosmos.network/t/amulet-security-advisory-for-cometbft-asa-2023-002/11604 is badly miscategorized, and should not have bene released publicly before a set of mitigations were made.  It isn't prudent to publicly release details about "amulet-security-advisory-for-cometbft-asa-2023-002" at this time because fixes are not yet in place.  However, it is necessary and safest to trend towards disclosure, so here is that plan:



## Plan

* release short term mitigations, this repository -- https://github.com/notional-labs/placid
* assist validators and chain teams with the implementation of mitigations
* work at a pace less than 16 hrs per day to ensure that issues in comet are addressed
* release solid fixes
* address procedural issues that caused the interchain foundation to bungle these items since 2021





## Mitigations 
these are the mitigations that I intend to ship today at 1400 GMT to the public on Amulet’s forum post, via twitter, and via DM/Telegram/Signal to chain teams in Notional’s Network.  That covers most, but not all of Cosmos.


### Have a block size between 2-5 mb

2mb blocks, not smaller, and be mindful of contract upload sizes for your chain, which may require larger blocks.  Don’t set block size to more than 5mb.
https://forum.cosmos.network/t/increase-maxblocksize-from-200k-to-2mb


### 100 gas per byte
* https://forum.cosmos.network/t/last-call-increase-the-size-of-the-memo-field-to-100kb-and-10x-the-cost-of-bytes/11500/11


### Jail validators faster

weather by adjusting signed blocks window downwards or min_signed_blocks_per_window upwards
https://forum.cosmos.network/t/adjust-min-signed-per-window-to-80/11808/1


### lower downtime slash

https://forum.cosmos.network/t/eliminate-the-downtime-slash-and-reduce-downtime-jail/11783/10


### use a globally consistent version of the mempool

* v1 seemed marginally better than v0



### decrease number of transactions the mempool can store

* from 5000 to a reasonable value i.e. 100 or 200 for now



### reduce the size of your chains mempool in bytes 

.  Tested on Cosmos hub replicated security testnet
Beware of Banana King transactions and consider a mempool filter for them until there is an updated IBC release
https://x.com/web3_analyst/status/1635687287962112000?s=20
https://github.com/cosmos/ibc-go/issues/4859
do not use x/globalfee to allow for gas bypasses
discontinue usage of x/globalfee
validators should enforce a globally consistent gas price using the configuration options for that.
use governance proposals to globally set a gas price, even if that cannot be enforced the way x/globalfee does, so that validators have a clear picture of what they should set gas prices to, instead of keeping that information unclear as it often is
Validators should ensure minimum 1gbps bandwidth and operate from bare metal nvme devices





## Credits

Mitigations and research were worked on by:

* Notional
  * Jacob
  * Khanh
  * Joe
  * Robin
  * Du
  * Vuong
  * Sheldon Dearr
  * Lit
* Cryptocrew
  * Clemens
* Iqlusion
  * Zaki
* Cosmos Hub
  * Jehan
* Hypha
  * Lexa
  * Udit
  * Dante
  * Denise
* Terraform Labs
  * Emi
* Osmosis
  * Dev
* Secret Network
  * Assaf
* Injective
  * Achilleas
* Celestia
  * Ismail
  * Luka
  * Eric
  * Callum
* Binary Builders
  * Marko
* Skip
* Range Security

Funding for cosmos hub related portions of this issue has been provided by the cosmos hub community pool via Proposal 104, and the AADAO grant to CryptoCrew.  No meaningful aid was provided by the Interchain Foundation, Amulet, or Informal Systems.

The issues mitigated here have been reported by Notional and others to the Interchain Foundation since 2021.
