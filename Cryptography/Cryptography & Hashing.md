# Ethereum Cryptography - Cheatsheet

> ❗ ECDSA (Elliptic curve digital signature algorithm) is used for signing messages (or transaction data) in the Ethereum blockchain

> ❗ An address in ethereum is essentially a hashed public key

> How do digital signatures work?
>
> The data that is wanting to be sent is first hashed using the sha-256 hashing function. That data is then signed with the author's private key and can be verified if it was sent by him using his public key.
>
> Whoever receives this signature can:
>
> 1) Derive the public key of the author
> 2) Verify that the message is the same as the one signed by the author
