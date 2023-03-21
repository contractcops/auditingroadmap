# Decenralized Applications(DApps)

Smart contracts are a way to decentralize the controlling logic and payment functions of applications.
Web3 DApps are about decentralizing all other aspects of an application: storage, messaging, naming, etc.

let’s take a closer look at the defining
characteristics and advantages of DApps.

What Is a DApp?
-
A DApp is an application that is mostly or entirely decentralized.
Consider all the possible aspects of an application that may be decentralized:
1. Backend software (application logic)
2. Frontend software
3. Data storage
4. Message communications
5. Name resolution

Each of these can be somewhat centralized or somewhat decentralized. For example, a
frontend can be developed as a web app that runs on a centralized server, or as a
mobile app that runs on your device. The backend and storage can be on private
servers and proprietary databases, or you can use a smart contract and P2P storage.

There are many advantages to creating a DApp that a typical centralized architecture
cannot provide:

>Resiliency
- >DApp backend will be fully distributed and managed on a blockchain platform. Unlike an application
deployed on a centralized server, a DApp will have no downtime and will continue to be available as long as the platform is still operating.

>Transparency
- >The on-chain nature of a DApp allows everyone to inspect the code and be more sure about its function. Any interaction with the DApp will be stored forever in the blockchain.

>Censorship resistance
- >As long as a user has access to an Ethereum node (running one if necessary), the user will always be able to interact with a DApp without interference from any centralized control. No service provider, or even the owner of the smart contract, can alter the code once it is deployed on the network.

Backend (Smart Contract)
-

Ethereum smart contracts allow you to build architectures in which a network of smart contracts call and pass data between each other, reading and writing their own
state variables as they go, with their complexity restricted only by the block gas limit.

One major consideration of smart contract architecture design is the inability to change the code of a smart contract once it is deployed. It can be deleted if it is programmed with an accessible SELFDESTRUCT opcode, but other than complete removal, the code cannot be changed in any way.

The second major consideration of smart contract architecture design is DApp size. A really large monolithic smart contract may cost a lot of gas to deploy and use. Therefore, some applications may choose to have off-chain computation and an external data source.

Having the core business logic of the DApp
be dependent on external data (e.g., from a centralized server) means your users will
have to trust these external resources.

Frontend (Web User Interface)
-
The client-side interface of a DApp can use standard web technologies (HTML, CSS, JavaScript, etc.). This allows a traditional web developer to use familiar tools, libraries, and frameworks.

>Interactions with Ethereum, such as signing messages, sending transactions, and managing keys, are
often conducted through the web browser, via an extension such as MetaMask.

The frontend is usually linked to Ethereum via the web3.js JavaScript library, which is bundled with the frontend resources and served to a browser by a web server.

Data Storage
-

Due to high gas costs and the currently low block gas limit, most DApps utilize off-chain data storage services, they store the bulky data off the Ethereum chain, on a data storage platform.

>That data storage platform can be centralized for example, a typical cloud database, or decentralize - stored on a P2P platform such as the IPFS, or Ethereum’s own Swarm platform.

>Decentralized P2P storage is ideal for storing and distributing large static assets

IPFS
-

IPFS stands for Inter-Planetary File System, it's a decentralized content-addressable storage system that distributes stored objects among peers in a P2P network.

>Content addressable means that each piece of content (file) is hashed and the hash is used to identify that file. You can then retrieve any file from any IPFS node by requesting it
by its hash.

IPFS aims to replace HTTP as the protocol of choice for delivery of web applications.
Instead of storing a web application on a single server, the files are stored on IPFS and
can be retrieved from any IPFS node.

Swarm
-

Swarm is another content-addressable P2P storage system, similar to IPFS. It was created by the Ethereum Foundation, as part of the Go-Ethereum suite of tools.

Like IPFS, it allows you to store files that get disseminated and replicated by Swarm nodes. You can access any Swarm file by referring to it by a hash. Swarm allows you to access a website from a decentralized P2P system, instead of a central web server.

Decentralized Message Communications Protocols
-
Another major component of any application is inter-process communication. That
means being able to exchange messages between applications, between different
instances of the application, or between users of the application.

>There are a variety of decentralized alternatives to server-based protocols, offering messaging over a P2P network.
The most notable P2P messaging protocol for DApps is Whisper, which is part of the Ethereum Foundation’s Go-Ethereum suite of tools.

