# Proxy pattern in smart contracts

> The proxy pattern (upgradeable smart contracts) is a pattern in Solidity programming that allows contracts to be written in such a way that if a bug is discovered in the future, the contract can be "upgraded", while the storage stays the same.

- Smart contracts are immutable, which means that once they are deployed onto the Ethereum blockchain, there is no viable way to change them. The only programmable option to directly alter a deployed contract is by using the SELFDESCTRUCT opcode which allows the contract to be removed from the blockchain.

![](assets/20230425_160551_image.png)

The proxy pattern should be used for potenially discovering bugs and being able to fix them/breaking changes with dependencies or specific OPCODES that are taken out of Solidity versions.

- Important to note that proxy/upgradeable contracts are not able to alter the originally deployed contract in any way. A use case would be to have a modifiable address state variable that points to a library contract.

The flow of the proxy pattern can be seen in the following diagram:

![](https://cdn.discordapp.com/attachments/367727894384082946/1100425795987583008/image.png)

- Proxy contracts can also be recognized as dispatchers that forward a transaction/call to a different module for its logic implementation.

<h4> Proxy - storage and context

The storage variables of upgradeable contracts can't be deleted/ommited (immutable), however, new storage variables might be added.

> <b> <b> <b>Important note: delegate contracts (module contracts) must have the same storage layout as the proxy contract. Changing this/ommiting this could cause a catastrophic fault/change in the state variables of the original proxy contract.

As for the context of a transaction, the following rules remain true:

- msg.sender is always the external contract/EOA that called the proxy, which in terms delegatecalled the logic/delegate contract. Therefore, once the transaction reaches the delegate, msg.sender would <b>**NOT** be the proxy contract.
- msg.value is also preserved, therefore any ether/bytes that is sent ends up at the delegate contract
- storage variables of the proxy are altered in the delegate contract

<h4> How is the proxy pattern implemented in Solidity

In Solidity (and other programming languages for that matter), modularity is a common practice that allows the seperation of business logic. Since smart contracts are immutable, we have to get creative in order to make a contract extendable.

In essence, the implementation of some business logic is done by a delegate contract. The address of this delegate contract is usually stored in a modifiable state variable inside the original proxy smart contract.

Ultimatelly, we want to recieve some data in the proxy contract and without modifying it - forward it to one of the proxy contracts that will then perform the necessary operations on it.

<h4> Pre-requisites

In order to best understand how proxies delegate information to a different contract, we need to understand how Solidity deals with "call data" to an external contract.

In Solidity, we are able to perform an external call to a contract with the following snippet:

````
(bool success,) = someContractAddress.call{value:123}(abi.encodeWithSignature("test(uint256)", 123))
````

With the above snippet of code, we are doing the following:

> Externally calling someContractAddress, with a specified value of 123 (wei) and the encoded function specifier, pointing to test(uint256) and a value of 123

Delegatecall has almost the exact same syntax, with the important difference of the preservation rules (mentioned above in this notebook).

````
(bool success,) = someContractAddress.delegatecall{value:123}(abi.encodeWithSignature("test(uint256)", 123))
````

Now, given all of this, it is not trivial to come to the conclusion that if we can somehow change the "someContractAddress" and the encoded call data that is being sent, we can achieve modularity and extend our proxy contract.

<h4> Implementation

It is important to note that delegatecall only returns a boolean of whether or not the function was succesfull or not. In order to overcome this, we can use some simple inline assembly in order to recieve further data from using a delegate contract.

To start things off, we look at Solidity's fallback function. The way that the EVM handles things in Solidity is that when msg.data of a transaction contains a function specifier that is not contained in a contract, it is forwarded to the fallback function of the called contract.

````cpp
fallback() external {
    assembly {
        let ptr: = mload(0x40)
        calldatacopy(ptr, 0, calldatasize())

        let result: = delegatecall(
            gas(),
            sload(implementation.slot),
            ptr,
            calldatasize(),
            0,
            0
        )

        let size: = returndatasize()

        returndatacopy(ptr, 0, size)

        switch result
        case 0 {
            revert(ptr, size)
        }
        default {
            return (ptr, size)
        }
    }
}
````

We use the above shown inline assembly in order to be able to recieve any kind of return output from the function we are delegatecalling to. Inline assembly is necessary here, as we do not know what kind of data will be returned in advance.

As explained in [Proxy pattern and upgradeable smart contracts](https://medium.com/@alvaro.serrano/proxy-pattern-and-upgradeable-smart-contracts-45d68d6f15da), we are able to bypass the need to use assembly if the delegate contract uses an event to broadcast the result. Since events cannot be listened to by smart contract themselves, the result would have to be directly interpreted by some frontend/something external of the Ethereum blockchain.
