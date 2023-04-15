# Storage vs memory

> In Solidity, 1 storage slot is exactly 256 bits (32 bytes). This means that in each storage slot, there can be 256 bits worth of variables packed into a single slot.

- Constants are not saved in storage, but compiled and "baked" into the bytecode of a smart contract

<h3> Storage

When the size of a given variable exceeds the storage slot's allocated 256 bits, it is moved onto the next slot. Currently, Solidity has 16 persistent storage slots, after which the compiler throws a "stack too deep" error.

In order to best understand this concept, we have the following example:

```solidity
uint16 a; //- STORED IN SLOT[0]
uint16 b; //- STORED IN SLOT[0]
address c; //- STORED IN SLOT[0]
uint64 d; //- STORED IN SLOT[0]
```

In the above shown example, we can see that the total size of all four variables amounts to exactly 256 bits, which means they get packed into one single storage slot. Storage can also be thought as the heap of a contract, where as memory is like the stack.

We have another variable:

```solidity
address a; // - STORED IN SLOT[0]
bool b; // - STORED IN SLOT[0]
uint256 c; //  - STORED IN SLOT[1]
```

We can see with the above example that the address and the boolean are both packed into one slot (they are 160 bits and 1 bit respectivelly). However, c takes up the whole new slot by itself, as it is a full 256 bit variable.

Finally, we have an example of how arrays and structs get stored into storage:

```solidity
uint256 a; // - STORED IN SLOT 0
struct SolidityStorage { // - ONLY DECLARATION, NOT STORED
	uint256 b;
	uint256 c;
}
SolidityStorage d; //SolidityStorage.b is in slot[1]
	           //SolidityStorage.c is in slot[2]
```

Dynamic types (such as string, which is just an array of characters, arrays, mappings) are only stored as the length of that variable. The actual "content" of the array/mapping/etc. is stored differently.

<h4> Function storage parameters

Storage parameters can be seen as simple pointers/references, alike to other programming languages such as C++, C, etc. Storage parameters are useful to allow libraries the access/ability to modify state variables.

<h3> Memory

Memory in Solidity can be seen as execution storage/stack storage. This means that it is only available for the execution thread, at the end of which it gets wiped.
