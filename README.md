# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos:

- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted.

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest is over and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest setup

## 🐺 C4: Set up repos

- [ ] Create a new private repo named `YYYY-MM-sponsorname` using this repo as a template.
- [ ] Get GitHub handles from sponsor.
- [ ] Add sponsor to this private repo with 'maintain' level access.
- [ ] Send the sponsor contact the url for this repo to follow the instructions below and add contracts here.
- [ ] Delete this checklist and wait for sponsor to complete their checklist.

## ⭐️ Sponsor: Provide contest details

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Name of each contract and:
  - [ ] lines of code in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Add all of the code to this repo that you want reviewed
- [ ] Create a PR to this repo with the above changes.

---

# ⭐️ Sponsor: Provide marketing details

- [ ] Your logo (URL or add file to this repo - SVG or other vector format preferred)
- [ ] Your primary Twitter handle
- [ ] Any other Twitter handles we can/should tag in (e.g. organizers' personal accounts, etc.)
- [ ] Your Discord URI
- [ ] Your website
- [ ] Optional: Do you have any quirks, recurring themes, iconic tweets, community "secret handshake" stuff we could work in? How do your people recognize each other, for example?
- [ ] Optional: your logo in Discord emoji format

---

# Contest prep

## 🐺 C4: Contest prep

- [ ] Rename this repo to reflect contest date (if applicable)
- [ ] Rename contest H1 below
- [ ] Add link to report form in contest details below
- [ ] Update pot sizes
- [ ] Fill in start and end times in contest bullets below.
- [ ] Move any relevant information in "contest scope information" above to the bottom of this readme.
- [ ] Add matching info to the [code423n4.com public contest data here](https://github.com/code-423n4/code423n4.com/blob/main/_data/contests/contests.csv))
- [ ] Delete this checklist.

## ⭐️ Sponsor: Contest prep

- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2021-06-gro/blob/main/README.md))
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 8 hours prior to contest start time.**
- [ ] Ensure that you have access to the _findings_ repo where issues will be submitted.
- [ ] Promote the contest on Twitter (optional: tag in relevant protocols, etc.)
- [ ] Share it with your own communities (blog, Discord, Telegram, email newsletters, etc.)
- [ ] Optional: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] Designate someone (or a team of people) to monitor DMs & questions in the C4 Discord (**#questions** channel) daily (Note: please _don't_ discuss issues submitted by wardens in an open channel, as this could give hints to other wardens.)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Trader Joe contest details

- $45,000 main award pot
- $2,500 gas optimization award pot
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-01-trader-joe-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts January 25, 2022 00:00 UTC
- Ends January 27, 2022 23:59 UTC

This repo will be made public before the start of the contest. (C4 delete this line when made public)

# Rocket Joe

_Rocket Joe_ is a token launch platform where participants bid to provide liquidity for newly issued tokens. The platform enables price discovery and token distribution over a period of time before tokens are issued to public market, while discouraging front-running by bots. In addition, it improves liquidity sustainability by allowing issuing protocols to acquire its own token liquidity.

Full whitepaper available [here](https://github.com/traderjoe-xyz/research/blob/main/RocketJoe_Launch_Platform_for_Bootstrapping_Protocol-Owned_Liquidity.pdf).

## How It Works

- User stakes JOE into the RocketJoeStaking contract to earn rJOE.
- LaunchEvent is created with a fixed amount of tokens to be issued.
- Users can deposit AVAX and rJOE into the LaunchEvent contract. The amount of rJOE needed to deposit a certain amount of AVAX is dictated by the parameter `rJoePerAvax`, which may vary from launch event to launch event.
- Users can also withdraw AVAX (if they think the price of TOKEN/AVAX is too high), but a withdrawal penalty may be incurred depending on which phase the launch event is at:

| Phase One  |                                   | Phase Two   | Phase Three                                |
| ---------- | --------------------------------- | ----------- | ------------------------------------------ |
| 0-24 hrs   | 24-48 hrs                         | 48-72 hrs   | Additional 0-7 days                        |
| 0% penalty | 0-50% penalty (linear increasing) | 20% penalty | LPs are locked + bonus incentives received |

- **Phase One**:
  - 0-24 hrs: Users can deposit and withdraw AVAX without any penalty.
  - 24-72 hrs: Users can continue to deposit and withdraw AVAX, but must incur a withdrawal penalty that increases linearly from 0-50% (the maximum is configurable).
- **Phase Two**: Users can _only_ withdraw AVAX with a 20% penalty (this parameter is also configurable).
- **Phase Three**: Initial liquidity is seeded, but the LP tokens are locked for an additional 0-7 days. As an incentive for locking, participants receive a bonus percentage of tokens once phase three starts. After this phase, both user and issuer are free to claim their LP tokens.

## Contracts

![Rocket Joe contract flow](assets/Rocket_Joe.jpeg)

###### RocketJoeToken

- 67 lines
- External contracts called: RocketJoeFactory
- Libraries used: ERC20, Ownable

An infinite supply ERC20 token that is an allocation credit for users to participate in a Launch Event. The amount of rJOE required to deposit an amount of AVAX into a launch event is dictated by `rJoePerAvax`, which is set manually on RocketJoeFactory. Once AVAX is deposited, rJOE is burned.

rJOE is also designed to be non-tradeable; we enforce this by defining our own `_beforeTokenTransfer` function in which you can only mint or burn but not transfer.

###### RocketJoeStaking

- 178 lines
- External contracts called: RocketJoeToken
- Libraries used: Initializable, OwnableUpgradeable, SafeERC20Upgradeable, IERC20Upgradeable

A MasterChef-style staking contract in which users stake JOE to earn rJOE.

###### RocketJoeFactory

- 245 lines
- External contracts called: RocketJoeToken, LaunchEvent, JoeFactory, JoeRouter, WAVAX
- Libraries used: Clones

Creates individual LaunchEvent contracts. Also sets `rJoePerAvax`.

###### LaunchEvent

- 621 lines
- External contracts called: RocketJoeToken, RocketJoeFactory, WAVAX, JoeRouter, JoePair, JoeFactory
- Libraries used: Ownable, IERC20Metadata

Contract in which price discovery and token distribution takes place. Issuer deposits the issued tokens and users deposit and/or withdraw AVAX during a 72 hour period. The final amount of AVAX at the end of this period dictates the TOKEN/AVAX price which will be used to the seed initial liquidity on the Trader Joe.

## Installation

The first things you need to do are cloning this repository and installing its
dependencies:

```sh
yarn
```

## Testing

To run the tests run:

```sh
make test
```

There is a pending bug with `solidity-coverage`. To get around this bug, you must manually edit `node_modules/solidity-coverage/plugins/hardhat.plugin.js` according to these [edits](https://github.com/sc-forks/solidity-coverage/pull/667/files).

Then to run coverage:

```sh
make coverage
```

The coverage report will then be found in `coverage/`.

## Deployment

### Rinkeby

To deploy to the [rinkeby network](https://www.rinkeby.io/) you need to set appropriate environment variables. The file [.env.example](.env.example) contains examples of the variables you need to set. For convenience you can copy this file to a file name _.env_ and use a tool like [direnv](https://direnv.net/) to automatically load it.

You could then deploy to rinkeby by using [hardhat-deploy](https://github.com/wighawag/hardhat-deploy) with this command `yarn hardhat deploy --network rinkeby`.

After the deploy is complete you should commit the _deployments_ directory to this repo.

### Verifying contracts

To verify the contracts on rinkeby you will need an etherscan API key, see [.env.example](.env.example). To verify a contract on you will need the deployed contracts address, run

```
yarn hardhat verify --network rinkeby "${contract_address}"
```

### License

Unlicensed
Copyright (c) 2022 Trader Joe - All rights reserved
