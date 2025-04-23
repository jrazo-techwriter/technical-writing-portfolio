# **Getting Started with the Solana JS SDK**

Overview
The Solana JavaScript SDK** (@solana/web3.js)** is a powerful library that allows developers to interact with the Solana blockchain directly from JavaScript-based applications. Whether you’re building a web-based dApp or working with **Node.js**, this guide will walk you through setting up your development environment, connecting to the network, and executing your first transaction.

---- 

## Prerequisites

Before diving in, make sure you have the following:
- Node.js (v16 or later recommended)
- npm or yam
- Basic familiarity with JavaScript/TypeScript
- A code editor (e.g., VSCode)
- Git installed and configured

---- 

## Step 1: Install the Solana JS SDK

You can install the SDK using either npm or yarn. **Npm install @solana/web3.js**\# or **Yarn add @solana/web3.js**
This pulls the latest stable version of the SDK into your project.


---- 

## Step 2: Connect to the Solana Network


	```Create a file named index.js and import the SDK.`
	Const solanaWeb3 = require (‘@solana/web3.js’); `
	// Connect to the Devnet`
	const connection = new solanaWeb3.Connection(`
	solanaWeb3.clusterApiUrl ( ‘devnet’ ), `
	);`
	console.log(“Connected to Solana Devnet”);`

Tip: You can switch between devnet, testnet, and mainnet-beta using `clusterApiUrl().`






---- 

## Step 3: Generate a New Wallet\\

Create a new keypair – this acts as your wallet.

	```Js
	```c`onst keypair = solanaWeb3. Keypair.generate ();`
	```console.log(“Public Key:”, keypair. publicKey. toBase58());`
`
`Store the keypair securely; it’s required to sign transactions.

---- 
## Step 4: Airdrop Soma SOL (Devnet Only)

Use the requestAirdrop() method to fund your wallet with test SOL.

	```js
	`(async () => {`
	`const airdropSignature = await connection.requireAirdrop(`
	`keypair. publicKey, `
	`solanaWeb3. LAMPORTS\_PER\_SOL`
	`);`
	await connection.confirmTransaction(airdropSignature);
	console.log(“Airdrop successful!”);
	`})();`





---- 
## Step: Send a Transaction

Let’s send 0.1 SOL from your new wallet to another address.

	```js
	`(async () => {`
	`const recipient = new solanaWeb3.PublicKey(‘RecipientPublicKeyHere`
	`const transaction = new solanaWeb3.Transaction().add(`
	`solanaWeb3.SystemProgram.transfer({`
	`fromPubkey: keypair.publicKey,`
	`toPubkey: recipient,`
	`lamports: 0.1 \* solanaWeb3.LAMPORTS\_PER\_SOL,`
	`})`
	);
	const signature = await `solanaWeb3.sendAndConfirmTransaction(`
	`connection, `
	`transaction,`
	`\[keypair]`
	`);`
	`console.log(“Transaction Signature:”, signature);`
	`})();`

NOTE: **Replace RecipientPublicKeyHere with a valid Solana address.

---- 

## Step 6: Verify on the Explorer
You can view the transaction using the Solana Explorer:

	```arduino
[https://explorer.solana.com/txt/\<TRANSACTION\_SIGNATURE\>?cluster=devnet](https://explorer.solana.com/txt/%3cTRANSACTION_SIGNATURE%3e?cluster=devnet)
Just replace \<TRANSACTION\_SIGNATURE\> with the one returned from your code.


---- 
##  Next Steps
Now that you’ve successfully:
- Installed the SDK
- Connected to the Devnet
- Generated a wallet
- Funded it via airdrop
- Sent a transaction
…you’re ready to explore Solana’s more advanced features like:
- Token creation
- Smart contract (program) deployment
- Wallet integrations (e.g., Phantom, Solflare)
Check out the full SDK docs:
👉🏼[https://solana-labs.github.io/solana-web3.js/](https://solana-labs.github.io/solana-web3.js/)
