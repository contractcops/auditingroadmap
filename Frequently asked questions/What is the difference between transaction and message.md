# What is the difference between transaction and message?

In the Ethereum protocol there's only transactions and message calls. A transaction is a type of message call.

As stated in the Yellow paper:

Transaction
-
A piece of data, signed by an External Actor. It represents either a Message or a new Autonomous Object. Transactions are recorded into each block of the blockchain.

>This means that a transaction represents either a Message or a new contract.

Message
-

Data (as a set of bytes) and Value (specified as Ether) that is passed between two Accounts, either through the deterministic operation of an Autonomous Object or the cryptographically secure signature of the Transaction

The following image explains this visually:

![Alt text](image/What%20is%20the%20difference%20between%20transaction%20and%20message/two-types-of-transactions.png)

Let me go in even more details:

A transaction may perform other message calls, but these are not transactions (even though blockchain explorers may label them inaccurately as "internal transactions"). These (internal) message calls are not published on the blockchain.

Let's illustrate this like a transaction in JavaScript:

    {
        from: ...,
        to: "C1",
        value: ...,
        gas: ...,
        data: ...,
        gasPrice: ...,
        nonce: ...
    }

This is what you will see on the blockchain: a "Normal transaction".

An "Internal Transaction" is the effects of taking the data part, feeding it to the contract C1, and executing the Ethereum Virtual Machine. The data is what tells C1 that it should call another contract C2: there is no separate {from:C1, to:C2,...} object on the blockchain that's needed.

Contracts calling each other, along with receiving payment, is the reason why "Normal transactions are usually much less than Internal transactions".