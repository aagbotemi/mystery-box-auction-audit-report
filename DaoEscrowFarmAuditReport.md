## DAO Escrow Farm Audit Report

1. A user could deposit more than 1 eth per block, by sending less than 1 ether in the same block multiple times. There is supposed to be a mapping to store how much is deposited per block.

2. For the re-entrancy, it is on line 52 and 49, state variable should be updated before transferring the refund value.

3. Optimizing the recieve function to save gas, it is recommended to use custom errors.