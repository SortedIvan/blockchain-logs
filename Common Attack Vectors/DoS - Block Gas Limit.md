# DoS with block gas limit

> Every OPCODE (different operations: arithmetic, storage, input output) has a predefined gas cost. This is to avoid the halting problem by Turing.

As of the writing of this attack, Ethereum has a block limit of aprox 30M.

There was a very famous ponzi contract called GovernMental where 1100 ethers got stuck because the block gas limit was not enough. 

The reason behind this is because to claim the ethers, the contract had to first delete a huge array of records, one that was too big for the gas limit back then.

A reference to governmental can be found here: [https://www.reddit.com/r/ethereum/comments/4ghzhv/governmentals_1100_eth_jackpot_payout_is_stuck/](https://www.reddit.com/r/ethereum/comments/4ghzhv/governmentals_1100_eth_jackpot_payout_is_stuck/)

There are many ways that DoS can occur with the block gas limit. Even though it is constantly growing and expanding, a few things should be kept in mind:

<h3> Preventative measures

Avoid creating contracts that have to do a lot of computation before accessing certain important logic. This can include:

- Long state arrays that have to be modified
- Too many external calls that have to happen at once
- The assigning/reassigning of a lot of state variables subsequently
