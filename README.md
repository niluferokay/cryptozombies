### Solidity Language Dictionary
\
**Uint** data type is an unsigned integer, it's value must be non-negative.\
**Structs** allow you to create more complicated data types that have multiple properties.\
**Arrays** are used when you want a collection of something:

- _string[5]: fixed array can contain 5 strings_
- _uint[]: dynamic array can contain many variables, have no fixed size_

**Public array** enables other smart contracts would be able to read from, but not write into it. useful for storing public data in your contract.\
**Array.push()** adds something to the end of the array.\
**Private function** names start with an underscore, the parameters inside the function as well:

- *_createZombies(_name, _dna)*

**View function** only views the data but not modifies it.\
**Pure function** disables access to any data in the app.

Ethereum has a hash function **keccak256** built in which is a version of SHA3. A hash function basically maps an input into a random 256-bit hex-decimal number. A slight change in the input will cause a large change in the hash. Keccak256 expects a single parameter of type bytes. This means that we have to "pack" any parameters before calling keccak256.

 - _keccak256(abi.encodePacked("abc"));_

**Events** are a way of your contract to communicate that something happened on the blockchain to your app front-end, which can be "listening" for certain events and take action when they happen (triggers).\
**Mapping** is another way of storing organized data(key-value pairs, dictionary).\
**Msg.sender** is a global variable that are available to all functions. It refers to the address of the person(or the smart contract) who have called the current function.

In Solidity, function execution always needs to start with an external caller. A contract will just sit on the blockchain doing nothing until someone calls one of its functions. So there will always be a msg.sender.

**Require** verifies certain conditions that must be true before running a function. If some condition is not true the function will throw an error and stop executing.\
**Inheritance** lets to split the code logic across multiple contracts to organize the code.\
**Storage** refers to variables stored permanently on the blockchain **memory** variables are temporarily stored and are erased between external function calls to the contract.\
**Internal** is the same as private, except that it's also accessible to contracts that inherit from the contract.\
**External** is similar to public except that these functions only be called inside that contract, they can't be called by other functions inside that contract.
