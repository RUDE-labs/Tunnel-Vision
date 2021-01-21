# Tunnel Vision

<p align="center">
  <img src="https://github.com/RUDE-labs/Tunnel-Vision/blob/master/images/Asset%2042%201.png" />
</p>


Tunnel Vision is a decentralized pay-as-you-go live streaming platform 📹. Decentralized transcoding is handled via [Livepeer](https://livepeer.org/) and micro-payments via the [Connext Network](https://connext.network/) built by dGuild for its transcoder [dGuild](https://explorer.livepeer.org/orchestrators).

This Tunnel Vision stream viewing dApp inspired by SpankCard (based on Dai Card) and Austin Griffith's burner wallet, Tunnel Vision is hosted in the browser, which utilizes Connext's Indra payment channels. 

For Dai Card-specific documentation please see the [Dai Card repo](https://github.com/ConnextProject/card).

## Contents
- [Development](#development)
    - [Local Development](#local-development)
    - [Testing Locally](#testing-locally)
- [Features](#features)
    - [Pre-funded Wallets](#pre-funded-wallets)

## Development

Prerequisites
 - Node 9+
 - Docker
 - Make

### Local development

1. Make sure you have indra running locally. Check out the instructions in the [indra repo](https://github.com/ConnextProject/indra).

TL;DR:

Ensure that you have the following installed:

- `make`: (probably already installed) Install w `brew install make` or `apt install make` or similar.
- `jq`: (probably not installed yet) Install w `brew install jq` or `apt install jq` or similar.
- [`docker`](https://www.docker.com/)
- [`node` and `npm`](https://nodejs.org/en/)

Then run:

```
git clone https://github.com/ConnextProject/indra.git
cd indra
npm start
```

2. Deploy

From the card's project root (e.g. `git clone https://github.com/ConnextProject/card.git && cd card`), run one of the following:

Using a containerized webpack dev server (recommended):
```
make start
```

Using a local webpack dev server:
```
npm install
npm start
```

The above step will take a while to completely finish because the webpack dev server takes a long time to wake up. Monitor it with:

```
bash ops/logs.sh server
```

3. Check it out

 - If you started with `npm start`, browse to `http://localhost:3000`
 - If you started with `make start`, browse to `http://localhost`
 - NOTE: If you started the card with `make` and you can view the Indra dashboard here: `http://localhost:3000/dashboardd/`

4. **(for Rinkeby development)** If you would like to use Indra on Rinkeby, ensure that `Rinkeby` is selected from the settings dialog (located in the upper right corner of the application). This will use the already-deployed Rinkeby Indra hub.

### Truffle Deployment

All multi-streams data is handled via the [`dTokStreams` contract](/contracts/dTokStreams.sol).

To compile the contract with Truffle, simply run (first, ensure that you have Truffle installed via `npm install -g truffle`):

```
truffle compile
```

To deploy the contract locally, you can run Ganache using: `ganache-cli` (if Ganache isn't installed yet, simply run: `npm install -g ganache-cli`). Run ganache-cli on network 4447:

```
ganache-cli -i 4447
```


Once Ganache is running, migrate the contracts using:

```
truffle migrate
```

[Via Drizzle](/src/index.js#L39), the [streamViewer component](/src/components/streamViewer.js#L741) automatically knows the deployment address of the contract.

### Testing locally

To run the tests during local development, start the test watcher with:

```
npm run start-test
```

This will start an ongoing e2e tester that will re-run any time the tests are changed. Works well with webpack dev server but you'll have to manually re-trigger the tests after changing the card's source code.

You can also run the more heavy-duty e2e tests that will be run as part of CI integration with:

```
npm run test
```

## Features

### Pre-funded Wallets

To maximise the ease of onboarding new users, prefunded [dai-card](https://daicard.io/) wallets can be used to generated and then shared. For example, if the prefunded wallet's mnemonic is the follow:

```
toilet civil kite grass little slogan critic whale guilt risk banner quarter
```

Then the following URL can be shared for a user to access the pre-funded wallet:

```
http://localhost/settings?mnemonic=toilet%20civil%20kite%20grass%20little%20slogan%20critic%20whale%20guilt%20risk%20banner%20quarter
```

Currently, these pre-funded wallets must be generated manually. (We plan to include a script for generating them programmatically in the future).
