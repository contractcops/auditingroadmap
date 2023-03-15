# Proof of stake

> ❗ What is proof of stake?

> Proof of stake is the consensus mechanism of Ethereum. It has replaced PoW as of 2022 and is 99.5% more energy efficient, secure and economically better for the Ethereum ecosystem.

> How does proof of stake work?
>
> Similar to PoW, proof of stake starts at the very beginning: the creation of a block. A block consists of validated transactions and new blocks are constantly added to the blockchain.
>
> In the previous PoW mechanism, miner nodes would compete trying to guess the nonce of the upcoming new block. This, however, is extremelly energy tasking as it requires running a lot of hardware at all times.
>
> With PoS, the hardware requirements are to have a normal computer that can run the 3 required pieces of software: an execution client, a consensus cliend and a validator. Furthermore, an economic requirement of 32 ETH is required to participate.

<h4> Validators in PoS

> What are validators?
>
> A validator is a pseudo-random selected node that creates the next block of transactions and sends it out to the other nodes. To participate as a validator, 32 ETH must be deposited into a "deposit" contract and join a queue (the queue is with a purpose so a fixed rate of new validators can exist).

> Quoted from the Ethereum documentation:
>
> "Whereas under proof-of-work, the timing of blocks is determined by the mining difficulty, in proof-of-stake, the tempo is fixed. Time in proof-of-stake Ethereum is divided into slots (12 seconds) and epochs (32 slots). One validator is randomly selected to be a block proposer in every slot. This validator is responsible for creating a new block and sending it out to other nodes on the network. Also in every slot, a committee of validators is randomly chosen, whose votes are used to determine the validity of the block being proposed."

> Gas tips go to block validators, base fee gets burned

<h4> Transactions in PoS

1) A user creates a transaction and signs it with his private key. This usually happens with a library such as ethers.js, web3.js, but under the hood its just making an API call to the JSON-RPC API

   > Here, users can specify a tip amount to give validators the incentive to include their transaction onto the next block
   >

2. The transaction is submitted to an Ethereum execution client which validates whether it has been signed with the correct private key and if they sender has enough ETH to make the transaction
3. Once validated (if valid) the transaction gets added to a local mempool of other pending transactions and is broadcasted over the network to other nodes. (gossip network).
4. One of the nodes is a validator (block propser). This node is selected pseudo-randomly depending on the amount of ether they are currently staking. The node is made up of 3 parts: execution client, validator and consensus client. The execution client bundles the transaction from the local mempool into an "execution" payload and executes them locally to apply the state change. Then this is passed to the consensus client where it is wrapped as part of a "beacon block" that has the rewards, penalties, slashings etc, to enable the network to agree.
5. Other nodes receive this beacon block on the consensus layer gossip network. They then check its validity and attest that the block is valid and that it is the next logical block that must be added. Then all nodes that attest add the block to their local database.
6. The transaction is "finalized". This means that it can't be reverted.
