# Chapter 7 - Smart Contracts & Solidity

> What are smart contracts? 
>
> To start, we go back to what EOA's are: client owned accounts that have an ether pool stored behind a private key. Smart contracts (or contract accounts) on the other hand do not have private keys and they "own" themselves.
>
> Smart contracts can also hold code and can execute functionality triggered by transactions. 
>
> Both type of accounts(contract and EOA's) have one thing in common - an address associated with them.

> What are smart contracts?
>
> - Computer programs: They are a piece of software that holds a certain functionality behind them. They are not legal documents, nor are they "smart".
> - Deterministic: Once created and deployed onto the blockchain, the functionality of a smart contract is pre-defined. This means that the outcome of a smart contract will be the same for everyone else, given that they execute it with the same context.
> - Immutable: Once deployed onto the Ethereum blockchain, a smart contract cannot be changed. Unlike normal software programs, a new instance of a smart contract must be deployed.
> - EVM context: smart contracts can execute based on a very limited execution context. They can access the transaction sender details, their own state and some information about the most recent blockss
> - Decentralized world computer: The EVN runs locally on every Ethereum node, but because all of the nodes operate under the same states and are always in sync, Ethereum operates as a single "world computer".


<h4> Lifecycle of a smart contract

> Smart contract lifecycle begins with the creation of one at the 0x0 address. 
>
> - The address of a smart contract is derived by the contract creation transaction as a function of the originating account and nonce.
> - The address of a smart contract can be used as a recipient, sending funds and executing functions
> - Smart contracts don't have keys

Smart contracts do not run autonomously or in the background. Their execution is always triggered by an incoming transaction. This means that they lie dormant until a transaction triggers one of their functionality.

> Smart contracts are not owned by anyone. This has to be specifically programmed inside a smart contract to specify that a specific address should be the owner of that contract.

- Smart contract execution is in no way parallel: the Ethereum computer should be considered a single-threaded machine.

> Transactions are atomic. This means that they execute one at a time and are only marked as succesful if a the functionality that is being execute terminates succesfully. The transaction is then recorded and is immutable.
>
> If execution fails due to an error, the functionality is "rolled back" and any state changes are removed. The transaction is still recorded as an attempt and the gas fees is taken from the originating account.

Smart contracts can't be changed. This, however, does not mean that smart contracts cannot be deleted. 

>  In order for a smart contract to be deleted, explicit functionality must be defined that allows the ability of the so called "SELFDESTRUCT" action. If the smart contract does not have this functionality it is immutable and can't be deleted.

<h4> High level languages for smart contracts

> ❗ In smart contracts, bugs literally cost money. This means that it is cruical for understand smart contracts and write them without any unintended effects.

> Declarative vs imperative languages:![1678905147096](image/Chapter7-Smartcontractsandsolidity/1678905147096.png)
>
> Declarative programming: declarative programming describes what you want the program to achieve rather than how it should achieve it.
>
> Examples: SQL, Lisp, Html, Prolog, Miranda
>
>
> Imperative programming: How the program should execute, given instructions and steps
>
> Examples: C++, Java, C# `<b>Solidity</b>`

Some of the more popular languages for smart contract programming are: 

- LLL - declarative programming language, rarely used anymore
- Serpent (imperative) like Python
- Solidity (imperative) like C++ and Java,Javascript
- Vyper (imperatative) like serpent
- Bamboo(imperative), no iterative loops and explicit state transitions. More auditable, but very new

>  Solidity comes with its own ABI (application binary interface) standart, as well as its ABI

<h4> The solidity ABI

Quoted from the book:

"In computer software, an application binary interface is an interface between two pro‐
gram modules; often, between the operating system and user programs. An ABI
defines how data structures and functions are accessed in machine code; this is not to
be confused with an API, which defines this access in high-level, often humanreadable formats as source code. The ABI is thus the primary way of encoding anddecoding data into and out of machine code"

<h5> Selecting a solidity compiler and language version

The Solidity language and compiler are still in constant flux and have regular updates. Normally, this would mean that the software version changes and contracts get outdated.

> To deal with this, Solidity offers a compiler direction known as a version pragma. This instructs the compiler that the program expects a specific compiler version:
>
> "pragma solidity ^0.4.19"

> Since Solidity programs are immutable and can't be changed (except deleted) new updates do not conflict with old contracts that use an older version of the language or the compiler.

<h4> Data Types

Referenced from the book: 

Boolean (bool)
Boolean value, true or false, with logical operators ! (not), && (and), || (or), ==
(equal), and != (not equal).
Integer (int, uint)


Signed (int) and unsigned (uint) integers, declared in increments of 8 bits from
int8 to uint256. Without a size suffix, 256-bit quantities are used, to match the
word size of the EVM.


Fixed point (fixed, ufixed)
Fixed-point numbers, declared with (u)fixedMxN where M is the size in bits
(increments of 8 up to 256) and N is the number of decimals after the point (up to
18); e.g., ufixed32x2.


Address
A 20-byte Ethereum address. The address object has many helpful member
functions, the main ones being balance (returns the account balance) and
transfer (transfers ether to the account).


Byte array (fixed)
Fixed-size arrays of bytes, declared with bytes1 up to bytes32.


Byte array (dynamic)
Variable-sized arrays of bytes, declared with bytes or string.

Enum
User-defined type for enumerating discrete values: enum NAME {LABEL1, LABEL
2, ...}.


Arrays
An array of any type, either fixed or dynamic: uint32[][5] is a fixed-size array of
five dynamic arrays of unsigned integers.


Struct
User-defined data containers for grouping variables: struct NAME {TYPE1
VARIABLE1; TYPE2 VARIABLE2; ...}.


Mapping
Hash lookup tables for key ⇒ value pairs: mapping(KEY_TYPE ⇒ VALUE_TYPE)
NAME.

Additionally, solidity has two more literals that can be used for the calculation of different units:

<h4> Time units

> The units seconds, minutes, hours, and days can be used as suffixes, converting
> to multiples of the base unit seconds

<h4> Ether units

> The units wei, finney, szabo, and ether can be used as suffixes, converting to
> multiples of the base unit wei.

<h4> Predefined global variables and functions

The next couple of snippets are directly taken from the book to avoid redundancy.

<h4> Transaction / message context

![1678906535331](image/Chapter7-Smartcontractsandsolidity/1678906535331.png)

<h4> Transaction context

![1678906570789](image/Chapter7-Smartcontractsandsolidity/1678906570789.png)

<h4> Block context

![1678906588244](image/Chapter7-Smartcontractsandsolidity/1678906588244.png)

<h4> Address object

![1678915615734](image/Chapter7-Smartcontractsandsolidity/1678915615734.png)

<h4> Contract, interface and libraries

> The main data (object) type in Solidity is a contract. A contract is much like a class in other languages - it encapsulates variables and functions.

> Library is a contract that is deployed once. It is to be used with the delegatecall() using its address

> Interface - just like a regular interface, the functions inside an interface are just definitions. Whatever contract inherits from an interface, it must implements those functions

<h4> Functions in Solidity

> Fallback functions: a function that has no name or access modifier. It also can't return anything. This function is called when no other function is specified in transaction data.

<h6> Access modifiers

1) public: This function can be called by any EOA or contracts, or from within the contract.
2) private: This function can only be called from within the contract.
3) external: EOA's and contracts can call these functions. These functions can't be called from inside the contract unless the keyword "this" is specified.
4) internal: These functions can't be called by EOA's. They can be called only from within the contract. Whenever a contract inherits (derives) from a contract with internal functions, they can be called.

> ❗ Important note: all functions/code are visible on the blockchain, no matter their access modifier.

<h6> Function behaviour modifiers

1. pure: When a function is pure, it promises the Solidity compiler that it does not read nor write any of the state variables. This is encouraged to be used in a declarative way with no side effects.
2. constant/view: When a function is specified as view, it promises that it might look into the state variables, but not write
3. payable: a function that can accept incoming payments

<h6> Constructor

Like other OOP languages, Solidity has a constructor which is ran only once.

> Setting up the msg.sender address (the address who creates the contract) as the owner of the contract
>
> ![1678916736191](image/Chapter7-Smartcontractsandsolidity/1678916736191.png)

> Allowing for deletion of the contract given that the originator address is the owner: 
>
> ![1678916765303](image/Chapter7-Smartcontractsandsolidity/1678916765303.png)

Solidity also has modifiers, which are usually used to "require()" some condition. They are very useful due to the fact that they can be easily reproduced and used in multiple functions without code duplication:

![1678916908533](image/Chapter7-Smartcontractsandsolidity/1678916908533.png)

<h4> Inheritance

Solidity supports inheritance from multiple contracts. 

<h4> Error handling

Transactions in Ethereum are atomic. This means that they are either executed and recorded onto the blockchain or the fail and all of the state changes are reverted until the point of execution.

In Solidity, you can use a "gate condition" by introducing a require statement. The require statement checks a condition and makes sure that it is either true or no code can execute under it.

> require(msg.value >= 0.1 ether, "Not enough ether!")

<h4> Events in Solidity

Every transaction, after its execution, creates a so called transaction receipt. The receipt contains log entries, which are constructed by the events in solidity.

It is generally good practice to introduce events when necessary, as it gives more information about a transaction and can provide context in case of an unexpected behaviour.

<h4> Calling other contracts (send, call, callcode, delegatecall)

> Calling other contracts can be potentially dangerous. Unless you are completely away of what a contract does and how it executes, you should avoid using functionality from foreign contracts. 

> The safest way to call a different contract is by creating it yourself. That way, you are aware of its functionality and are certain that it will behave as expected.
>
> To do this, we can instantiate it using the "new" keyword, just like in other OOP languages. 

> Another possibility is to cast the address of an existing instance of the contract. This way you are consuming its interface of an already existing instance. 
>
> ❗ It is critically important that the instance you are using is of the type you assume. Furthermore, this method is much less safe than the above, as its more prone to mistakes and you are not completely sure of its functionality.

![1679000081723](image/Chapter7-Smartcontractsandsolidity/1679000081723.png)

<h5> Raw call, delegatecall

> These are the most "low-level" methods in Solidity of contract to contract communication. 
>
> ❗ This is also the most flexible and dangerous way of communication.

> ❗ Using <contract_adr>.call("method", parameters) is a blind call into a function. It can expose the contract to multiple security risks, but most importantly reentrancy.

> Another variant of call is delegatecall(). It is different in a way that it the msg context does not change. 
>
> For example, whereas call() changes the value of msg.sender to be the calling contract, delegatecall() keeps the same msg.sender as in the calling contract. delegatecall() runs the code of another contract inside the context of the execution of the current contract. It is most often used to invoke code from a library.

> call() changes the value of msg.sender to be the calling contract, delegatecall() keeps the same msg.sender as in the calling contract.

call() shown as 1)

delegatecall() as 2)

![1679001762967](image/Chapter7-Smartcontractsandsolidity/1679001762967.png)


> Important note:
>
> tx.origin != msg.sender
>
> The transaction origin(tx.origin) is the EOA that initiated the transaction.
>
> The msg.sender can be any contract/EOA that invoked the function



> DELEGATECALL: 
>
> https://solidity-by-example.org/delegatecall/

> ❗❗ Difference between CALL() and DELEGATECALL()

![1679003189446](image/Chapter7-Smartcontractsandsolidity/1679003189446.png)

![1679003220134](image/Chapter7-Smartcontractsandsolidity/1679003220134.png)
