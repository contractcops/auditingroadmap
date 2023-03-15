# Common attacks with contract/EOA addresses

<h4> address.send(amount) - contract

> address.send(amount) returns a boolean, false on error. This should always be checked and handled appropriately.

<h4> address.call(payload) - contract

> Low level CALL function - it can construct a message call with a data payload. Returns a boolean, false on error. This function is unsafe: recipient can accidentally or maliciously use up all of your gas, causing your contract to halt with OOG exception. Always check the result of this function.
