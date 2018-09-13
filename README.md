<p align="center">
  <img width="350px" src="http://dutchx.readthedocs.io/en/latest/_static/DutchX-logo_blue.svg" />
</p>

# DutchX Example: How to use the CLI
This project is just an example on how to use the CLI, and also is meant to be
used for interacting with the DutchX.

> The CLI was created for testing porpoused only, so some command might work
> different than expected.
>
> So please comment any issues with the CLI so we can
> improve it over time.

# Documentation
Checkout the [DutchX Documentation](http://dutchx.readthedocs.io/en/latest).

# Get started with the CLI
First install docker:
* [https://docs.docker.com/install/]()

Then execute the following steps:

```bash
# Clone repo
git clone https://github.com/gnosis/dx-examples-cli.git dx-cli
cd dx-cli

# Allow the CLI script to be executed
chmod +x cli

# Run help to get all the commands avalible
#   NOTE: The 1st time you run the command, it will take a litle time because
#    it must download the Docker image.
./cli -h
```
# Config
The configuration is done using environment variables.
The `cli` script is already configured and should work as it is, but in order to
do trading you should change the mnemonic to use your own.

The best way to redefine the mnemonic or any other config, is just creating a
`local.conf` file that override the environemnt variabled defined in the `cli`.

> This local.conf is ignored by git, so is ok to have your personal config there.

Example of `local.conf`:

```bash
MNEMONIC="myth like bonus scare over problem client lizard pioneer submit female collect"
MARKETS=WETH-RDN
ETHEREUM_RPC_URL=https://rinkeby.infura.io
NETWORK=rinkeby
```

# Basic usage
## Get account balances
Get balances for all tokens of an account
```bash
# Show the balances of the first account of the MNEMONIC
./cli balances

# You can ask for the balances of any other account
./cli balances --account=The desired account
```
## Get token list
Get all avaliable tokens in the DX
```bash
./cli tokens
```

## Get markets
Get all the token pairs in the DX
```bash
./cli markets
```

## State of the auction
Get information about the current auction of a given token pair.
```bash
./cli state WETH-RDN
```

## Get price
There are several prices, depending on what we need:

```bash
# Current price of an ongoing auction
#   This price is the price biders use (it's going down)
#   A N/A means there is no price (i.e. an auction that didn't run, so  you
#   cannot bid).
./cli price WETH-RDN

# Price in USD for a number of tokens
#   It will use the closing price and the Ether oracle
./cli usd-price 1 RDN

# Closing prices
#   It'll show the last N closing prices
./cli closing-prices WETH-RDN


# Price of an external exchange (i.e. 'binance', 'huobi', 'kraken', 'bitfinex')
#   This is not part of the DX. It's just a reference.
./cli market-price WETH-RDN
```

## Get auctions
Show the information of the auctions that cleared in a given period of time.

Filter by dates:

```bash
# Get todays auctions
./cli auctions --period today

# Get last 7 days auctions
./cli auctions --period week

# Get this week's auctions
./cli auctions --period week

# Get last week's auctions
./cli auctions --period last-week

# Get auctions between two dates
./cli auctions --from-date=25-05-2018 --to-date=26-05-2018
```

You can export the result into a CSV file (it'll include some extra info):
```bash
# Export the result to a file
./cli auctions --from-date=25-05-2018 --to-date=26-05-2018 --file=auctions.csv
```

## Get trades

Show the information of the trades that were executed in a given period of time.

```bash
# Get todays auctions
./cli trades --period today

# Get last 7 days auctions
./cli trades --period week

# Get this week's auctions
./cli trades --period week

# Get last week's auctions
./cli trades --period last-week

# Get auctions between two dates
./cli trades --from-date=25-05-2018 --to-date=26-05-2018
```

Aditionally, you can applu any of this filters:
```bash
# Filter by token
#   It will filter by trades of auctions that contain the given token
./cli trades --period today --token RDN

# Filter by sell token
#   It will filter by trades of auctions that contain the given token as a sell
#   token
./cli trades --period today --token RDN

# Filter by buy token
#   It will filter by trades of auctions that contain the given token as a buy
#   token
./cli trades --period today --token RDN

# Filter by auction index
#   It will filter by trades of auctions that contain the given token as a buy
#   token
./cli trades --period today --auction-index 24

# Filter by account
#   It will filter by trades of the given account address
./cli trades --period today --auction-index 24
```

You can export the result into a CSV file (it'll include some extra info):
```bash
# Export the result to a file
./cli trades --from-date=25-05-2018 --to-date=26-05-2018 --file=auctions.csv
```

# Trade in the DX
To be able to trade, you have to edit the `cli` file and modify the `MNEMONIC`
variable and use your own mnemonic.

The best way to redefine the mnemonic, is just creating a `local.conf` file in
the root of the project with the following content:
```
MNEMONIC='this is your secret mnemonic that will be ignored by git'
```

## Send tokens
This method is not part of the DX, but it cames handy for testing.

It will send tokens ERC20 from one account to another.

```bash
# Send 0.8 RDN to a given account
./cli send 0.8 RDN 0x627306090abab3a6e1400e9345bc60c78a8bef57
```

# Deposit and Withdraw
You can deposit tokens and withdraw them by using the following commands:

```bash
# Deposit WETH
#   It will wrap Ether automatically if you don't have enough WETH balance
./cli deposit 0.35 WETH

# Deposit any other token
#   Same way :)
./cli deposit 12 RDN


# Withdraw
./cli withdraw 0.35 WETH
./cli withdraw 12 RDN
```

# Trade in the DX
```bash
# Sell
#   Sell 0.1 WETH in the next WETH-RDN auction
./cli sell 0.1 WETH-RDN

# Buy
#   Buy 12 RDN in the current auction
./cli buy 12 WETH-RDN

# Claim the seller tokens
#   Once the auction clears
./cli claim-seller WETH-RDN

# Claim the buyer tokens
#   Once the auction clears
./cli claim-buyer WETH-RDN
```

# Security and Liability
All the code is provided WITHOUT ANY WARRANTY; without even the implied warranty
 of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

# Feedback, suggestions, collaborations
Please, let us know any typo or error in the project or documentation.

Any idea, proposal or colaboration will be welcome.

Also, you are encouraged to participate in the [Gitter Channel for the DutchX](https://gitter.im/gnosis/DutchX).

# Contributors
- Stefan ([Georgi87](https://github.com/Georgi87))
- Martin ([koeppelmann](https://github.com/koeppelmann))
- Anxo ([anxolin](https://github.com/anxolin))
- Dani ([dasanra](https://github.com/dasanra))
- Dominik ([dteiml](https://github.com/dteiml))
- David ([W3stside](https://github.com/w3stside))
- Dmitry ([Velenir](https://github.com/Velenir))
- Alexander ([josojo](https://github.com/josojo))
