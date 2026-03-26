# Foundry FundMe Project 🚀

A decentralized crowdfunding smart contract built with Solidity. This project demonstrates how to bridge the gap between deterministic blockchains and real-world data using Chainlink Oracles.

---

## 📌 Project Overview

The **FundMe** contract allows users to contribute funds (ETH) to a project with a built-in requirement: the contribution must meet a minimum USD value. Since Ethereum's price is volatile, the contract fetches real-time price data from the **Chainlink Decentralized Oracle Network**.

### Key Technical Highlights

- **Oracle Integration:** Resolves the "Oracle Problem" by fetching off-chain data securely on-chain.
- **Data Scaling:** Implements math to convert Chainlink's 8-decimal price feed into Ethereum's 18-decimal Wei standard.
- **Gas Efficiency:** Uses `view` functions for read-only operations to ensure zero gas costs for external calls.
- **Interface Architecture:** Utilizes `AggregatorV3Interface` for modular and clean contract design.

---

## 🛠 Features & Functions

### 1. `getPrice()`
Fetches the current price of ETH/USD from the Sepolia Testnet.

- **The Logic:** Chainlink returns the price with 8 decimals. The function scales this by `1e10` to match the 18 decimals of Wei.
- **Type Casting:** Safely converts `int256` from the oracle to `uint256`.

```solidity
function getPrice() public view returns (uint256) {
    (, int256 answer, , , ) = priceFeed.latestRoundData();
    return uint256(answer * 1e10);
}
```

---

### 2. `getVersion()`
Returns the version of the Chainlink Aggregator contract being used (currently **version 4** on Sepolia).

This ensures the contract is interacting with the correct and latest Chainlink logic.

```solidity
function getVersion() public view returns (uint256) {
    return AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306).version();
}
```

---

### 3. `fund()`
The entry point for contributions.

> ⚠️ Currently implementing the `getConversionRate()` logic to dynamically validate the $5 USD minimum against the `msg.value` in Wei.

---

### 4. `latestRoundData()` — From Chainlink
Returns key oracle data:

| Return Value | Description |
|---|---|
| `roundId` | The round in which the price was updated |
| `answer` | The actual ETH/USD price (8 decimals) |
| `startedAt` | Timestamp when the round started |
| `updatedAt` | Timestamp of last price update |
| `answeredInRound` | Round in which the answer was computed |

---

## 🌍 Network & Deployment

The contract is deployed and tested on the **Ethereum Sepolia Testnet**.

| Contract / Interface | Address |
|---|---|
| ETH/USD Price Feed | `0x694AA1769357215DE4FAC081bf1f309aDC325306` |
| Network | Sepolia Testnet |

---

## 🚀 How to Run

### 1. Clone the repo
```bash
git clone https://github.com/AhmedHawash321/chainlink-fundme-project.git
cd chainlink-fundme-project
```

### 2. Compile with Remix
- Open `FundMe.sol` in [Remix IDE](https://remix.ethereum.org)
- Ensure compiler version is `^0.8.24`

### 3. Deploy
- Use **Injected Provider (MetaMask)**
- Switch to **Sepolia Testnet**
- Hit **Deploy** and confirm the transaction

---

## 🧠 Key Concepts Learned

| Concept | Description |
|---|---|
| Oracle Problem | Blockchains are deterministic — they can't access external data natively |
| Chainlink | Decentralized oracle network that bridges on-chain and off-chain data |
| `AggregatorV3Interface` | Interface used to interact with Chainlink price feeds modularly |
| Wei Conversion | Chainlink returns 8 decimals → multiply by `1e10` to get 18 decimal Wei |
| `view` functions | Read-only, consume zero gas when called externally |
| `interface` in Solidity | Defines function signatures without implementation — enables contract interaction |

---

## 📝 Learning Journey

This project is part of my deep dive into **Web3 & Blockchain Development**, following the **Cyfrin Updraft Full Blockchain Course** by Patrick Collins.

### Progress
- [x] Solidity Basics — Storage, Structs, Mappings
- [x] Contract Inheritance & Factories
- [x] Chainlink Oracle Integration (this project)
- [ ] Foundry Testing & Deployment Scripts
- [ ] DeFi Protocol Development

**Next Step:** Transitioning to **Foundry** for advanced testing and deployment scripting.

---

## 📄 License

This project is licensed under the **MIT License**.
