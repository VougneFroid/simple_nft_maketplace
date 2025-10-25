# NFT Marketplace on Sui Blockchain

A complete NFT marketplace smart contract built with Move for the Sui blockchain. This project enables minting, listing, buying, and managing NFTs with an integrated marketplace system.

## 🚀 Features

### Core NFT Functions
- **Mint NFTs** - Create unique NFTs with name, description, and image URL
- **Update Description** - Modify NFT descriptions after minting
- **Burn NFTs** - Permanently delete NFTs from the blockchain
- **Transfer NFTs** - Send NFTs to other addresses

### Marketplace Functions
- **List NFTs** - Put your NFTs up for sale at a specified price
- **Buy NFTs** - Purchase listed NFTs with SUI tokens
- **Cancel Listings** - Remove your NFTs from the marketplace
- **Marketplace Fees** - Automatic 2% marketplace fee collection
- **Fee Withdrawal** - Admin function to withdraw collected fees

### Developer Features
- **Entry Functions** - Easy interaction from wallets and IDEs
- **Public Functions** - Composable functions for Programmable Transaction Blocks (PTBs)
- **Getter Functions** - Read NFT and listing data on-chain
- **Event Emission** - Track all marketplace activities through events
- **Comprehensive Tests** - Full test suite included

## 📋 Prerequisites

- [Sui CLI](https://docs.sui.io/build/install) installed
- Sui wallet with testnet SUI tokens
- Basic understanding of Move programming language

## 🛠️ Installation

1. Clone the repository:
```bash
git clone https://github.com/tzarumang/simple_nft_marketplace.git
cd simple_nft-marketplace
```

2. Build the project:
```bash
sui move build
```

3. Run tests:
```bash
sui move test
```

4. Publish to Sui testnet:
```bash
sui client publish --gas-budget 100000000
```

## 📖 Usage

### Minting an NFT

**From CLI:**
```bash
sui client call \
  --package <PACKAGE_ID> \
  --module nft_marketplace \
  --function mint_to_sender \
  --args "My First NFT" "This is my first NFT on Sui" "https://example.com/image.png" \
  --gas-budget 10000000
```

**From TypeScript SDK:**
```typescript
const tx = new Transaction();
tx.moveCall({
  target: `${packageId}::nft_marketplace::mint_to_sender`,
  arguments: [
    tx.pure.string("My First NFT"),
    tx.pure.string("This is my first NFT on Sui"),
    tx.pure.string("https://example.com/image.png"),
  ],
});
```

### Listing an NFT for Sale

```bash
sui client call \
  --package <PACKAGE_ID> \
  --module nft_marketplace \
  --function list_nft_for_sale \
  --args <NFT_OBJECT_ID> 1000000000 \
  --gas-budget 10000000
```
*Note: Price is in MIST (1 SUI = 1,000,000,000 MIST)*

### Buying an NFT

```bash
sui client call \
  --package <PACKAGE_ID> \
  --module nft_marketplace \
  --function buy_nft \
  --args <LISTING_OBJECT_ID> <PAYMENT_COIN_ID> <MARKETPLACE_OBJECT_ID> \
  --gas-budget 10000000
```

### Canceling a Listing

```bash
sui client call \
  --package <PACKAGE_ID> \
  --module nft_marketplace \
  --function cancel_listing \
  --args <LISTING_OBJECT_ID> \
  --gas-budget 10000000
```

## 🏗️ Smart Contract Structure

### Structs

#### `DevNetNFT`
```move
public struct DevNetNFT has key, store {
    id: UID,
    name: string::String,
    description: string::String,
    url: Url,
}
```

#### `Listing`
```move
public struct Listing has key {
    id: UID,
    nft_id: ID,
    price: u64,
    seller: address,
}
```

#### `Marketplace`
```move
public struct Marketplace has key {
    id: UID,
    balance: Balance<SUI>,
}
```

### Events

- `MintNFTEvent` - Emitted when an NFT is minted
- `ListNFTEvent` - Emitted when an NFT is listed
- `PurchaseNFTEvent` - Emitted when an NFT is purchased
- `DelistNFTEvent` - Emitted when a listing is canceled

### Entry Functions (For External Calls)

| Function | Description |
|----------|-------------|
| `mint_to_sender` | Mint and transfer NFT to sender |
| `update_nft_description` | Update NFT description |
| `burn_nft` | Permanently delete an NFT |
| `list_nft_for_sale` | List NFT on marketplace |
| `buy_nft` | Purchase a listed NFT |
| `cancel_listing` | Cancel/delist an NFT |
| `withdraw_marketplace_fees` | Withdraw collected fees (admin) |

### Public Functions (For Composability)

| Function | Description |
|----------|-------------|
| `mint` | Create and return NFT |
| `update_description` | Update NFT description |
| `burn` | Delete an NFT |
| `list_nft` | List NFT for sale |
| `purchase_nft` | Buy NFT (returns excess payment) |
| `delist_nft` | Cancel listing |

### Getter Functions (View Functions)

| Function | Description |
|----------|-------------|
| `name` | Get NFT name |
| `description` | Get NFT description |
| `url` | Get NFT image URL |
| `nft_id` | Get NFT object ID |
| `listing_price` | Get listing price |
| `listing_seller` | Get seller address |
| `listing_nft_id` | Get NFT ID from listing |
| `listing_id` | Get listing object ID |
| `marketplace_balance` | Get marketplace fee balance |

## 💰 Fee Structure

The marketplace automatically collects a **2% fee** on all NFT sales:
- **98%** goes to the seller
- **2%** goes to the marketplace balance

Fees can be withdrawn by calling `withdraw_marketplace_fees`.

## 🧪 Testing

Run the test suite:
```bash
sui move test
```

### Test Coverage
- ✅ NFT minting and transfers
- ✅ Description updates
- ✅ NFT burning
- ✅ Getter functions
- ✅ Entry function interactions

## 📝 Example Workflow

1. **Alice mints an NFT**
   ```bash
   mint_to_sender("Cool Art", "My artwork", "https://ipfs.io/...")
   ```

2. **Alice lists the NFT for 5 SUI**
   ```bash
   list_nft_for_sale(nft_id, 5000000000)
   ```

3. **Bob buys the NFT**
   ```bash
   buy_nft(listing_id, payment_coin, marketplace)
   ```
   - Bob pays 5 SUI
   - Alice receives 4.9 SUI (98%)
   - Marketplace receives 0.1 SUI (2%)
   - NFT ownership transfers to Bob

4. **Bob updates the description**
   ```bash
   update_nft_description(nft_id, "Now owned by Bob!")
   ```

## 🔐 Security Considerations

- **Price validation** - Listings must have price > 0
- **Payment verification** - Payment must be >= listing price
- **Seller authorization** - Only sellers can cancel their listings
- **Automatic refunds** - Excess payment is returned to buyer
- **Event tracking** - All marketplace actions emit events

## 📚 Resources

- [Sui Documentation](https://docs.sui.io/)
- [Move Language](https://move-language.github.io/move/)
- [Sui TypeScript SDK](https://sdk.mystenlabs.com/typescript)
- [Sui Explorer](https://suiexplorer.com/)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

Apache-2.0 License

## 👥 Authors
@tzarumang

Created for Move In Campus Activity

## 🐛 Bug Reports

If you discover any bugs, please create an issue on GitHub with detailed information.

## ⚠️ Disclaimer

This is educational software. Use at your own risk. Always audit smart contracts before deploying to mainnet.

---

**Happy Building on Sui! 🌊**
