# Function selector abuse

Function selector is how we tell them smart contract which function to run. It is the first 4 bytes which we get from the functions name and it is used when calling low level functions like call on a contract and asking it to run a function.

>The function selector is most probably unique for a contract. It is just 4 bytes and that is not a lot of data and thus there can exist another function which has a totally different name but same selector. But that is not that important.

Let's observe this example:

![Alt text](image/Function%20selector%20abuse/how%20to%20get%20functionSelector.png)

For example if I try to get the function selector of 

    "getSelector(string)"

I get:

    0x80a003ff

All ABI-compliant contracts on Ethereum begin by looking at these bytes of the calldata and jump to the corresponding function body.

If a contract performs a call to an external contract, and the user can influence any part of the method signature (such as the function name, or its type), they can call any function on the external contract, simply by manipulating the string until a selector matching the desired one is found.