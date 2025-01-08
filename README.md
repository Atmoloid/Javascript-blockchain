# Simple Blockchain in JavaScript

This project is a simple blockchain implementation written entirely in JavaScript. It serves as an educational tool to understand the basic concepts behind blockchain technology, including blocks, hashes, and chain validation. This implementation is not intended for production use but is a great starting point for learning!

## Features

- **Blocks**: Each block contains an index, timestamp, data, previous hash, hash, and nonce.
- **Hashing**: Uses `SHA256` for creating secure hashes for block data.
- **Genesis Block**: Automatically creates the first block of the chain.
- **Validation**: Ensures the integrity of the chain through a `checkValid()` method.

---

## Code Overview

### Block Class
The `Block` class represents individual blocks in the blockchain.

```javascript
class Block {
    constructor(timestamp, data) {
        this.index = 0;
        this.timestamp = timestamp;
        this.data = data;
        this.previousHash = "0";
        this.hash = this.calculateHash();
        this.nonce = 0;
    }

    calculateHash() {
        return SHA256(this.index + this.previousHash + this.timestamp + this.data + this.nonce).toString();
    }

    mineBlock(difficulty) {
        // Mining logic will go here
    }
}
```

#### Methods:
- **`calculateHash()`**: Computes the hash of the block using the block's data, timestamp, and nonce.
- **`mineBlock(difficulty)`**: Placeholder for implementing Proof of Work.

---

### Blockchain Class
The `Blockchain` class manages the chain of blocks.

```javascript
class Blockchain {
    constructor() {
        this.chain = [this.createGenesis()];
    }

    createGenesis() {
        return new Block(0, "01/01/2017", "Genesis block", "0");
    }

    latestBlock() {
        return this.chain[this.chain.length - 1];
    }

    addBlock(newBlock) {
        newBlock.previousHash = this.latestBlock().hash;
        newBlock.hash = newBlock.calculateHash();
        this.chain.push(newBlock);
    }

    checkValid() {
        for (let i = 1; i < this.chain.length; i++) {
            const currentBlock = this.chain[i];
            const previousBlock = this.chain[i - 1];

            if (currentBlock.hash !== currentBlock.calculateHash()) {
                return false;
            }

            if (currentBlock.previousHash !== previousBlock.hash) {
                return false;
            }
        }

        return true;
    }
}
```

#### Methods:
- **`createGenesis()`**: Creates the first block of the chain, known as the "Genesis Block".
- **`latestBlock()`**: Retrieves the most recent block in the chain.
- **`addBlock(newBlock)`**: Adds a new block to the chain, ensuring it links correctly to the previous block.
- **`checkValid()`**: Verifies the integrity of the chain by checking hashes and links between blocks.

---

## Example Usage
Here's a simple example of how to use the blockchain:

```javascript
let jsChain = new Blockchain();
jsChain.addBlock(new Block("12/25/2017", {amount: 5}));
jsChain.addBlock(new Block("12/26/2017", {amount: 10}));

console.log(JSON.stringify(jsChain, null, 4));
console.log("Is blockchain valid? " + jsChain.checkValid());
```

This will:
1. Create a new blockchain with a genesis block.
2. Add two new blocks to the chain.
3. Print the chain structure to the console.
4. Verify the validity of the chain.

---

## Dependencies
This implementation uses the [CryptoJS](https://www.npmjs.com/package/crypto-js) library for SHA256 hashing. To install it, run:

```bash
npm install crypto-js
```



