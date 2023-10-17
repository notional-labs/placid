# placid
mitigations to https://forum.cosmos.network/t/amulet-security-advisory-for-cometbft-asa-2023-002/11604, which was released against my express wishes by [Amulet](https://twitter.com/amuletdotdev), the security contractor to [Interchain Foundation](https://interchain.io).


## Notes

https://forum.cosmos.network/t/amulet-security-advisory-for-cometbft-asa-2023-002/11604 is badly miscategorized, and should not have bene released publicly before a set of mitigations were made.  It isn't prudent to publicly release details about "amulet-security-advisory-for-cometbft-asa-2023-002" at this time because fixes are not yet in place.  However, it is necessary and safest to trend towards disclosure, so here is that plan:



## Plan

* release short term mitigations, this repository -- https://github.com/notional-labs/placid
* assist validators and chain teams with the implementation of mitigations
* continue working like mad to ensure that all ecosystem modules are patched for banana king
* work at a pace less than 16 hrs per day to ensure that issues in comet are addressed
* release solid fixes
* release full documentation on what p2p storms are, and their intersection with banana king
* address procedural issues that caused the interchain foundation to bungle these items since 2021











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

Funding for cosmos hub related portions of this issue has been provided by the cosmos hub community pool via Proposal 104, and the AADAO grant to CryptoCrew.  No meaningful aid was provided by the Interchain Foundation, Amulet, or Informal Systems.

The issues mitigated here have been reported by Notional and others to the Interchain Foundation since 2021.
