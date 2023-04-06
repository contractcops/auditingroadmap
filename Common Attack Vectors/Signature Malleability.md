# Signature Malleability

Surprisingly, signatures can be altered without the possession of the private key and still be valid.

That's what we are going to observe here.

The EVM specification defines several so-called 'precompiled' contracts one of them being

    ecrecover

ecrecover() is a very useful Solidity function that allows the smart contract to validate that incoming data is properly signed an expected party.

It just executes the elliptic curve public key *recovery*.

So what is the vulnerability you may ask?

A malicious user can slightly modify the three values *v*, *r* and *s* to create other valid signatures.

A system that performs signature verification on contract level might be susceptible to attacks if the signature is part of the signed message hash.

>Valid signatures could be created by a malicious user to replay previously signed messages.

Let's look at some code to spot where is exactly the problem:


    function transfer(
            bytes _signature,
            address _to,
            uint256 _value,
            uint256 _gasPrice,
            uint256 _nonce)
        public
        returns (bool)
    {
      bytes32 txid = keccak256(abi.encodePacked(getTransferHash(_to, _value, _gasPrice, _nonce), _signature));
      require(!signatureUsed[txid]);

      address from = recoverTransferPreSigned(_signature, _to, _value, _gasPrice, _nonce);

      require(balances[from] > _value);
      balances[from] -= _value;
      balances[_to] += _value;

      signatureUsed[txid] = true;
    }

As you can see, here on this line:

      bytes32 txid = keccak256(abi.encodePacked(getTransferHash(_to, _value, _gasPrice, _nonce), _signature));

the txid is including the _signature in the message hash to check if previously messages have been processed by this contract. That where the vulnerability lies.

But let's now focus on the ECDSA to understand the full picture.

Here’s the curve that is used for Ethereum (& BTC), which is Secp256k1.

![Alt text](image/Signature%20Malleability/secp256k1Curve.png)

Secp256k1 defines a group of points, they are basically the points on the elliptic curve, and for each (x, y) combination x && y are elements of the field Zp, where Zp is a field for p prime.

This curve uses the following formula

![Alt text](image/Signature%20Malleability/curveFormula.png)

    p - prime number(a very large number)
    n - order of the group(number of points on the EC)


Now, when we are signing a transaction, *we are making new random private key which is temporary*

This is extremely important to understand, becuase we want to achieve non-determinism in the ECDSA so the reverse engineering of the private key is impossible!

After this, we are using this this *temporary private key point* to generate a corresponding P on the EC.

    P = G,
    where G is the generator point
    (G = {x = 55066263022277343669578718895168534326250603453777594175500187360389116729240, y = 32670510020758816978083085130507043184471273380659243275938904335757337482424}),
    k is your temporary private key (a number in the range [1...n-1])

This P is the public key point that corresponds with our *temporary private key k*

Remember, P is a point. So from this P, we get the x-coordinate as our r value. In other words:

    r = P's x coordinate

We are ready to calculate the signature proof. Let's see what is the formula:

![Alt text](image/Signature%20Malleability/signatureProofFormula.png)


    Where:
    s - the signature proof,
    z - the message hash,
    r - P's x coordinate,
    e - Our real private key,
    k - our temporary private key.
    n - prime order of the secp256k1 group

And if I can simplify this for you:

    s = ((messageHash + P's x coordinate*your private key) + k^(-1)) (mod n)

And there you go, now we have the *r* and *s* values

But now, let's return for a bit to the *r* value, like I said it was the x coordinate of the temporary public key.

Let's visualize this point on the curve now:

![Alt text](image/Signature%20Malleability/pointPonTheCurve.png)

But if you think about it, the point P isn't the only point on the curve that has the same x'coordinate.

If we draw stright line from the x coordinate of point P up to the other side of the EC, we will observe that this straight line will intersect the EC at the exact x coordinate as point P.

So in a nutshell we will have other point P, let's call it P2 that has the same x coordinate, but with other y coordinate.

Let's take a look:

![Alt text](image/Signature%20Malleability/pointP2onTheCurve.png)

And here comes variable *v* in to the game.

The ECDSA has to know which point was used when creating the signature proof *s* value so that the signer address can be recovered.

This is where the value *v* comes in to play, it simply acts as an indicator for which side of the EC the actual public key that was used is on.

And here comes the vulnerability.

Vulnerability
-

After we now have the two points P2 and P, we can compute a signature proof value for both of them hence we can make a double spending from a contract