## Mystery Box Auction Audit Report

1. The "addAuthorizedBidder()" function should be an external function and at least a public function.

2. The "auctionInProgress" which is a state variable boolean, is not set to true anywhere, therefore, it should set to true between line 35 and line 36 - This is a DOS attack.

3. Line 34 requires that the contract balance should be 100000000000, and there is no function to fund the wallet for the price pool, except for low level can, even with that, fallback() and recieve() function are not available in the contract

4. Line 40 in the endAuction() function requires that block.number > auctionStartTime + 7 days. Here, block.number is being compared with block.timestamp

5. Line 47, tx.origin should be msg.sender because it is susceptible to attack.

6. Line 52. The key of the winning mapping was set to tx.origin which is susceptible to attack. msg.sender is better to use. A malicious contract could exploit the tx.origin, by getting an address that has a balance in the winnings mapping to

7. Line 54. the winning[tx.origin] should be winnings[msg.sender] and it should be set to zero. Also this line should have come before paying out the price to prevent re-entrancy attack

8. In the withdrawWinnings() function, there's no check that the msg.sender is the topBidder and the mysteryPrize isn't being sent to the topBidder

9. The withdrawWinnings() function is sending money to recieve which may be a random address

10. The function addAuthorizedBidder(address _authUser) uses an array, a mapping would have been better. Using mapping will optimise gas when the size of the array gets bigger.

11. In the bid() function, there is no where to check if the bid of the next bidder is greater than the bid of the previous bidder.

12. The startAuction() function can be called by anyone who is not an admin.

13. The endAuction() function did not set the auctionInProgress to false.

14. In the mysteryPrize() function, the random number *randomHash* should be gotten from a decentralized Oracle like Chainlink VRF. A calling contract can exploit the poor randomness of randomHash by using the global variable block.difficulty, block.timestamp during the call to generate the randomHash in the calling contract and if the outcome is most profitable

