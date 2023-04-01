# Reentrancy Attacks

Reentrancy Attack repeatedly withdraws funds from a smart contract, let's call it Vulnerable Contract and transfers them to other smart contract until the funds of the Vulnerable Contract the  have been exhausted.

Now let's go through some examples:

This is a typical example of contract that is vulnerable to single function reentrancy attack

![1680345906808](image/ReentrancyAttacks__/1680345906808.png)

Do you see that

    balances[msg.sender] -= _amountToWithdraw;

is executed after

    msg.sender.call{value: _amountToWithdraw}("");
Where is the problem here you may ask?
msg.sender.call will invoke the fallback function in the contract that invoke the withdraw function.

If some contract has the fallback function and in this function implements again the withdraw function of EtherStore contract, a devastating recursion will occur and will drain the EtherStore's balance.

Here is an example of particular attack:

![1680345915317](image/ReentrancyAttacks__/1680345915317.png)

When the attacker calls the attack function in the Attack contract:

1. Deposits 1 ether
2. Immediately withdraw the ether
3. In the fallback function in the Attack contract again invokes the withdraw function
4. Repeat that process until the balance of the EtherStore is equal >= 1 ether.
