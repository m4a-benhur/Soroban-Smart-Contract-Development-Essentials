# Soroban Smart Contract Development Essentials

## What is a Smart Contract? 🤔
A **smart contract** is like a robot 🤖 that follows rules you write in code and runs on a **blockchain**. With **Soroban**, we can write smart contracts that work on the **Stellar blockchain**! 🌍✨

---

## 1. Setting Up Soroban 🛠️

Before we start, we need to install Soroban. Click the link below and follow how:

https://developers.stellar.org/docs/build/smart-contracts/getting-started/setup

## 2. Writing Your First Soroban Smart Contract 📜

Let's create a simple **Hello, Blockchain!** contract. It will return a greeting message! 👋🌍

1. Create a new project:

```sh
mkdir hello_soroban
cd hello_soroban
cargo init --lib
```

2. Open `Cargo.toml` and add Soroban:

```toml
[dependencies]
soroban-sdk = "20.0.0"
```

3. Open `src/lib.rs` and write this:

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, vec, Env, String, Vec};

#[contract]
pub struct Contract;

#[contractimpl]
impl Contract {
    pub fn hello(env: Env, to: String) -> Vec<String> {
        vec![&env, String::from_str(&env, "Hello"), to]
    }
}
```

### What’s Happening Here? 🤔
- `#![no_std]` – Makes our contract lightweight.
- `use soroban_sdk::*;` – Brings in Soroban tools.
- `#[contract]` – Marks this as a smart contract.
- `pub fn hello(env: Env, to: String) -> Vec<String>` – A function that returns `"Hello"` plus a name.

---

## 3. Building and Deploying the Contract 🚀

### Build the Contract
Run this to compile:

```sh
cargo build --release --target wasm32-unknown-unknown
```

This creates a **WASM file**, which is how Soroban runs contracts!

### Deploy the Contract
To deploy, use Soroban CLI:

```sh
soroban contract deploy --wasm target/wasm32-unknown-unknown/release/hello_soroban.wasm
```

This will return a **contract ID** (keep it safe!).

### Invoke the Contract
Run this to call `hello`:

```sh
soroban contract invoke --id <YOUR_CONTRACT_ID> --fn hello --arg "World"
```

Output:

```
["Hello", "World"]
```

🎉 Congrats! You've built your first Soroban smart contract!

---

## Conclusion 🎯

Great work! ✅ You just:
- Installed Soroban CLI
- Created a **Hello, Blockchain!** contract
- Built and deployed it

Soroban makes it easy to build smart contracts on **Stellar**! Next, you can explore:
- Handling **payments** 💰
- Creating **tokens** 🎟️
- Building **decentralized apps (dApps)** 🏗️

---


## Counter Smart Contract for Soroban 

Imagine you have a **magic notebook** 📖 that remembers a number. Every time you press a button, the number **goes up by 1**. That’s exactly what this smart contract does! It keeps track of a **counter** that increases each time we call the `increase` function.

---

### Here’s the Smart Contract Code:  

```rust
#![no_std]  
use soroban_sdk::{contract, contractimpl, Env, Symbol};  

#[contract]  
pub struct CounterContract;  

#[contractimpl]  
impl CounterContract {  
    // Function to get the current counter value  
    pub fn get(env: Env) -> u32 {  
        env.storage().instance().get(&Symbol::new(&env, "count")).unwrap_or(0)  
    }  

    // Function to increase the counter by 1  
    pub fn increase(env: Env) {  
        let mut count = Self::get(env.clone()); // Get the current count  
        count += 1; // Add 1 to it  
        env.storage().instance().set(&Symbol::new(&env, "count"), &count); // Save it back  
    }  
}
```

---

## What’s Happening Here? 🤔  

Let’s break it down **step by step**, like a **video game tutorial!** 🎮

### 1️⃣ **`#![no_std]` – No Standard Library 🚀**
This line tells Rust **not to use** the standard library. Why? Because **smart contracts** need to be **lightweight** and **run on the blockchain**, so we keep things small.

---

### 2️⃣ **`use soroban_sdk::{contract, contractimpl, Env, Symbol};` – Bringing in the Tools 🛠️**
- `soroban_sdk` is a **toolbox** that helps us write smart contracts.
- `contract` and `contractimpl` help define a **smart contract**.
- `Env` is a special object that lets us **interact with the blockchain**.
- `Symbol` is like a **label** that helps us store data.

---

### 3️⃣ **`#[contract]` – Declaring the Smart Contract 📜**
```rust
#[contract]  
pub struct CounterContract;
```
- Here, we **create** our smart contract and name it `CounterContract`.
- It’s like **naming a new game character** before playing!

---

### 4️⃣ **`#[contractimpl] impl CounterContract` – The Brain of the Contract 🧠**
This is where we **write functions** (like mini-programs) to make the contract do things!

---

### 5️⃣ **Getting the Current Count (`get` function) 🔍**
```rust
pub fn get(env: Env) -> u32 {  
    env.storage().instance().get(&Symbol::new(&env, "count")).unwrap_or(0)  
}
```
- The `get` function **reads** the current counter value.
- It checks a **special storage box** on the blockchain for a value labeled `"count"`.
- If it finds a number, it **returns it**! If there’s **nothing there yet**, it returns `0` (starting from zero).

---

### 6️⃣ **Increasing the Counter (`increase` function) 🔢**
```rust
pub fn increase(env: Env) {  
    let mut count = Self::get(env.clone()); // Get the current count  
    count += 1; // Add 1 to it  
    env.storage().instance().set(&Symbol::new(&env, "count"), &count); // Save it back  
}
```
- First, we **get** the current count (from storage).
- Then, we **add 1** to it (just like pressing a game score button 🎮).
- Finally, we **save** the new number back into storage so the blockchain remembers it.

---

## How This Works in Real Life 🌍
Imagine you build this smart contract and **deploy** it on the blockchain. Here’s what happens when you use it:

1️⃣ **You call `get` → It returns `0` (because no number is stored yet).**  
2️⃣ **You call `increase` → It sets the count to `1`.**  
3️⃣ **You call `get` again → Now, it returns `1`.**  
4️⃣ **You keep calling `increase`, and the number keeps going up! 🚀**  

---

## Why is This Useful? 🤔
This is a **basic example** of how blockchain **remembers things**! You could use this to:
- **Count how many times someone interacts** with a system.  
- **Track votes in an election** 🗳️.  
- **Keep a leaderboard in a game** 🎮.  
- **Store points in a reward system** 🏆.  

---

## What’s Next? 🚀
Now that you understand this simple contract, try adding:
✅ A **reset function** to set the counter back to 0.  
✅ A **decrease function** to subtract 1.  
✅ A **limit** so the number doesn’t go below 0.  

---

### Final Thought 💡  
This contract is **like a magic notebook 📖 that remembers a number forever!** Every time someone calls `increase`, the number **goes up** and stays there on the blockchain.

Congratulations, young blockchain developer! 🏆🎉 Keep building and changing the world with smart contracts! 🚀🔥
