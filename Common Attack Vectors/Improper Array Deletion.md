# Improper Array Deletion

Although removing an element from an array in Solidity can be simple, there are several complex components hidden behind it.

If we take a look at the following example, we will observe that we have a delete function

![Alt text](image/Improper%20Array%20Deletion/etherRemoval.png)

Let's say I invoke the function, by deleting the second item in the array. After this I invoke the getLnegth() function to see the length of the array.
And interestingly enough, the size of the array stays the same.

Why is that?

Because Solidity will just replace the value of the deleted index with zero. Hence the length of the array isn't modified.

Best practices for Array Deletion in Solidity
-

Instead of delete or <array_name>.length = 0, use push and pop functions to interact with the array elements.

1. We can remove array elements by shifting them from right to left.

![Alt text](image/Improper%20Array%20Deletion/functionRemoveFromRightToLeft.png)

2. We can remove an array element by copying the last element into the place to remove

![Alt text](image/Improper%20Array%20Deletion/functionRemoveLastElement.png)