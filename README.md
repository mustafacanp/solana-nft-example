# Create a Solana NFT collection

Used the following 3 repositories:

https://github.com/HashLips/hashlips_art_engine - Generate assets

https://github.com/metaplex-foundation/metaplex - Upload to decentralized storage and deploy assets

https://github.com/exiled-apes/candy-machine-mint - Mint NFTs

# 1. `hashlips_art_engine`

1.1. Configure your collection from the `src/config.js`

1.2. `yarn generate_metadata` - Generate assets (assets will be generated at `hashlips_art_engine/build/images` and `hashlips_art_engine/build/json`)

# 2. `metaplex`

2.1. Move assets to the `metaplex/assets`(except `_metadata.json`) (png and JSON files should be in the same folder)

2.2. [Choose your network](#2.2.-choose-network)

2.3. [Create and set keypair](#2.3.-create-and-set-keypair)

2.4. [Upload assets](#2.4.-upload-assets)

2.5. [Verify assets](#2.5.-verify-assets)

2.6. [Deploy assets](#2.6.-deploy-assets)

2.7. [Update price or live date](#2.7.-update-price-or-live-date)

2.8. [Get the Important Info](#2.8.-get-the-important-info)

# 3. `candy-machine-mint`

3.1. [Minting Process](#3.1.-minting-process)

## 2.2. Choose Network

### Devnet

```bash
solana config set --url https://api.devnet.solana.com
```

### Mainnet

```bash
solana config set --url https://api.mainnet.solana.com
```

## 2.3. Create and Set keypair

Create and Set keypair - Wallet Config

```bash
solana-keygen new --outfile ~/.config/solana/devnet-hashlips.json
```

```bash
solana-keygen help new
```

`KEYPAIR_PATH=".../solana/metaplex/~/.config/solana/devnet-hashlips.json"`

```bash
solana config set --keypair (KEYPAIR_PATH)
```

### Install and run metaplex repository

```bash
cd metaplex/js
yarn install
yarn build
yarn bootstrap
```

## 2.4. Upload Assets

Upload assets(png) with metadata(json) to the decentralized web via Arweave (`upload`)

```bash
npx ts-node js/packages/cli/src/candy-machine-cli.ts upload ./assets --env devnet --keypair (KEYPAIR_PATH)
```

Check the following path to see metadata of uploaded tokens:
`.../solana/metaplex/.cache`

Uploaded asset example on the [arweave.net](https://fybshk64ltrtox2ubuuwzw5jsqbbgwxs45z5lyu2h7kmtulnz2va.arweave.net/LgMjq9xc4zdfVA0pbNuplAITWvLnc9Ximj_UydFtzqo/)

## 2.5. Verify Assets

Verify that upload was successful (`verify`)

```bash
λ npx ts-node js/packages/cli/src/candy-machine-cli.ts verify --keypair (KEYPAIR_PATH)
```

## 2.6. Deploy Assets

Deploy assets (`create_candy_machine`)

```bash
npx ts-node js/packages/cli/src/candy-machine-cli.ts create_candy_machine --env devnet --keypair (KEYPAIR_PATH) -p 0.2
```

## 2.7. Update Price or Live Date

Update the `price` / `go_live_date` (`update_candy_machine`)

```bash
npx ts-node js/packages/cli/src/candy-machine-cli.ts update_candy_machine --keypair (KEYPAIR_PATH) --price 0.4 --date "19 Oct 2021 00:00:00 EST"
```

## 2.8. Get the Important Info

Get the important info from `metaplex/.cache/devnet-temp`

```json
{
  "program": {
    "config": "3ttSunuUAQwt6gxuJgiLWCBwaqYQKqkKfXFh6ZawbTV1",
    ...
  },
  "authority": "H4e7PvwrUDUhGXWoJupHQCTuwPNJDBSSh9BJvUNLzZdW",
  "candyMachineAddress": "25BJ4FQVS8Huv1HvdyQVS2y6k6uNSAXLtgnckFZze9zn",
  "startDate": 1637989200,
  ...
}
```

# 3. `candy-machine-mint`

## 3.1. Minting Process

Create an `.env` file in the `candy-machine-mint` repository with the `Important Info`.
Installing packages and start the application,

```bash
yarn install && yarn start
```

Mint your NFTs!

### References:

https://www.youtube.com/watch?v=35RO0lAEIxE
https://docs.metaplex.com/create-candy/introduction
https://docs.solana.com/cli/usage
