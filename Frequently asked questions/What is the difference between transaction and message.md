# What is the difference between transaction and message?

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