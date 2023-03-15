# Smart Contracts and Solidity

What Is a Smart Contract?
-
> immutable computer programs that run deterministically in the context of an
Ethereum Virtual Machine as part of the Ethereum network protocol—i.e., on the
decentralized Ethereum world computer.


- >Computer programs
Smart contracts are simply computer programs. The word “contract” has no legal
meaning in this context.

- >Immutable
Once deployed, the code of a smart contract cannot change. Unlike with tradi‐
tional software, the only way to modify a smart contract is to deploy a new
instance.

- >Deterministic
The outcome of the execution of a smart contract is the same for everyone who
runs it, given the context of the transaction that initiated its execution and the
state of the Ethereum blockchain at the moment of execution.

- >EVM context
Smart contracts operate with a very limited execution context. They can access
their own state, the context of the transaction that called them, and some infor‐
mation about the most recent blocks

- >Decentralized world computer
The EVM runs as a local instance on every Ethereum node, but because all
instances of the EVM operate on the same initial state and produce the same final
state, the system as a whole operates as a single “world computer.”

Life Cycle of a Smart Contract
-
Contracts only run if they are called by a transaction. All smart contracts
in Ethereum are executed, ultimately, because of a transaction initiated from an EOA.

A contract can call another contract that can call another contract, and so on, but the
first contract in such a chain of execution will always have been called by a transac‐
tion from an EOA. Contracts never run “on their own” or “in the background.”

- > Successful termination means that the program executed without an
error and reached the end of execution.

- > If execution fails due to an error, all of its
effects (changes in state) are “rolled back” as if the transaction never ran.

A failed
transaction is still recorded as having been attempted, and the ether spent on gas for
the execution is deducted from the originating account, but it otherwise has no other
effects on contract or account state.

It is important to remember that a contract’s code cannot
be changed.
However, a contract can be “deleted,” removing the code and its internal
state (storage) from its address, leaving a blank account.
Any transactions sent to that
account address after the contract has been deleted do not result in any code execu‐
tion, because there is no longer any code there to execute.

>To delete a contract, you
execute an EVM opcode called SELFDESTRUCT

>That operation costs “negative gas,” a gas refund, thereby incentivizing the release of network
client resources from the deletion of stored state

>If the contract’s code does not have a SELFDESTRUCT opcode, or it is inaccessible,
the smart contract cannot be deleted.

Introduction to Ethereum High-Level Languages
-
The EVM is a virtual machine that runs a special form of code called EVM bytecode,
analogous to your computer’s CPU, which runs machine code such as x86_64.Most Ethereum developers use a high-level language to write programs, and a compiler to convert them into bytecode.

While any high-level language could be adapted to write smart contracts, adapting an
arbitrary language to be compilable to EVM bytecode is quite a cumbersome exercise
and would in general lead to some amount of confusion.

- >LLL
A functional (declarative) programming language, with Lisp-like syntax. It was
the first high-level language for Ethereum smart contracts but is rarely used
today.

- >Serpent
A procedural (imperative) programming language with a syntax similar to
Python. Can also be used to write functional (declarative) code, though it is not
entirely free of side effects.

- >Solidity
A procedural (imperative) programming language with a syntax similar to Java‐
Script, C++, or Java. The most popular and frequently used language for Ethereum smart contracts.

- >Vyper
A more recently developed language, similar to Serpent and again with Pythonlike syntax. Intended to get closer to a pure-functional Python-like language than
Serpent, but not to replace Serpent.

- >Bamboo
A newly developed language, influenced by Erlang, with explicit state transitions and without iterative flows (loops). Intended to reduce side effects and increase auditability. Very new and yet to be widely adopted.

