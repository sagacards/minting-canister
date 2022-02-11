# Minting Canister

This canister holds minting authority over the NFT canister, and is solely responsible for minting (i.e. no NFT can be minted by means other than the minting canister.)

The minting canister will expose the following avenues for minting an NFT.
- Before any sales, but not during or after, a list of `admins` may manually mint.
- When put into `presale` mode, NFTs may be minted by principals listed on a preconfigured `whitelist` by paying the preconfigured `presale price`.
- When put into `public sale` mode, NFTs may be minted by any principal by paying the preconfigured `public sale price`.

## Purchase flow

Not intended to be prescriptive necessarily, but this should illustrate the high-level:

- Principal requests a lock
    - Assert whitelist if presale
    - Assert available unminted
    - Create lock (on a specific token index)
    - Create invoice
    - Return invoice and lock
- Principal completes payment using invoice canister
    - Complete payment using invoice canister (???)
- Principal calls `settle` on minting canister
    - Minting canister validates the lock
    - Minting canister validates the invoice / purchase
    - Minting canister mints the token and removes the old lock


## Assessing the invoice canister

Using the Dfinity invoice canister will create an additional (potentially cross subnet) set of update and query calls, and transaction speed is critical to the purchase experience. It might for that reason, or for some other reason, be less attractive than a custom solution. We may even still decide to use it until such a solution could be built.

## Requirements, assumptions, test cases, etc.

- Mints may not exceed supply.
- Transaction locks be valid for five minutes.
- Minting canister must expose admin-only configuration functionality:
    - Set NFT canister: principal
    - Set pre sale price: e8s nat
    - Set public sale price: e8s nat
    - Set whitelist: (principal, count, note)
    - Set mode: static | presale | public sale
    - Get config: read all config data in one request
- The minting canister may only hold mint authority over a single NFT canister at any given time, but it may be reconfigured arbitrarily

## Bounty

The minting canister shall receive 10% of proceeds from minting sales of the Legends NFTs, subject to some change. This will be split amongst contributors to the minting canister.