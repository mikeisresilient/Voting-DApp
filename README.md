

# Ballot Voting DApp

A decentralized voting application built on Ethereum. This project allows a chairperson to give voting rights, users to vote on proposals, and queries the winning proposal. It also includes Node.js scripts for handling **Bytes32 strings** using `ethers.js`.

---

## **Project Structure**

```
transform/
├── contracts/
│   └── Ballot.sol           # Solidity smart contract for voting
├── scripts/
│   ├── parseBytes.js        # Converts bytes32 to string
│   └── createBytes.js       # Converts string to bytes32
├── package.json             # Node.js project config for ethers.js
└── README.md
```

---

## **Smart Contract: Ballot.sol**

The `Ballot` contract allows:

* Chairperson to give voting rights.
* Voters to vote for proposals.
* Querying the winning proposal and name.

**Key Structures:**

```solidity
struct Voter {
    uint weight;
    bool voted;
    uint vote;
}

struct Proposal {
    string name;
    uint voteCount;
}
```

**Key Variables:**

* `Proposal[] public proposals` – List of proposals.
* `mapping(address => Voter) public voters` – Tracks voters.
* `address public chairperson` – Creator of the ballot.

---

### **Key Functions**

| Function                                     | Description                                                |
| -------------------------------------------- | ---------------------------------------------------------- |
| `constructor(string[] memory proposalNames)` | Sets up the ballot with proposals and assigns chairperson. |
| `giveRightToVote(address voter)`             | Chairperson grants voting rights to an address.            |
| `vote(uint proposal)`                        | Voter casts a vote for a proposal.                         |
| `winningProposal()`                          | Returns the index of the winning proposal.                 |
| `winningName()`                              | Returns the name of the winning proposal.                  |

**Rules:**

* Only the chairperson can give voting rights.
* Each voter can vote only once.
* Votes are weighted (chairperson initially has weight 1).

---

## **Node.js Scripts**

The project uses `ethers.js` to handle bytes32 string conversions:

### **1. parseBytes.js**

Converts `bytes32` to human-readable string:

```bash
node parseBytes.js <bytes32_value>
```

Example:

```bash
node parseBytes.js 0x4d7950726f706f73616c00000000000000000000000000000000000000000000
# Output: "MyProposal"
```

---

### **2. createBytes.js**

Converts a string to `bytes32`:

```bash
node createBytes.js "MyProposal"
```

Output:

```text
0x4d7950726f706f73616c00000000000000000000000000000000000000000000
```

---

## **Installation**

1. Clone the repository:

```bash
git clone https://github.com/mikeisresilient/Voting-DApp.git
cd Voting-DApp/transform
```

2. Install Node.js dependencies:

```bash
npm install
```

3. Compile the contract (using Hardhat or Remix):

```bash
npx hardhat compile
```

---

## **Deployment**

Deploy the Ballot contract:

```bash
npx hardhat run scripts/deploy.js --network localhost
```

Or deploy to a testnet (e.g., Goerli):

```bash
npx hardhat run scripts/deploy.js --network goerli
```

---

## **Testing**

Run tests using Hardhat:

```bash
npx hardhat test
```

---

## **Technologies Used**

* Solidity ^0.8.0
* Hardhat for contract development & testing
* Node.js & ethers.js for bytes32 string parsing
* Ethereum-compatible blockchain (local or testnet)

---

## **How to Use**

1. **Deploy the Contract**

   * Deploy `Ballot.sol` via **Hardhat**, **Remix**, or any Ethereum environment.
   * Pass an array of proposal names during deployment:

     ```js
     ["Proposal1", "Proposal2", "Proposal3"]
     ```

2. **Grant Voting Rights**

   * Only the chairperson can give voting rights:

     ```solidity
     giveRightToVote(voterAddress)
     ```

3. **Cast Votes**

   * Voters can vote by specifying the proposal index:

     ```solidity
     vote(proposalIndex)
     ```

4. **Query Results**

   * Find the winning proposal index:

     ```solidity
     winningProposal()
     ```
   * Get the winning proposal name:

     ```solidity
     winningName()
     ```

5. **Using Node.js Scripts** (Optional)

   * Convert a string to `bytes32` for contract interaction:

     ```bash
     node createBytes.js "ProposalName"
     ```
   * Convert `bytes32` back to a string:

     ```bash
     node parseBytes.js <bytes32_value>
     ```


## **Notes**

* Chairperson starts with voting weight 1.
* Voters must be granted rights before voting.
* The Node.js scripts help convert proposal names to and from `bytes32` for smart contract interactions.

