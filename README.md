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

**View function** only views the data but not modifies it. View functions don't cost any gas when they're called externally by a user. This is because view functions don't actually change anything on the blockchain – they only read the data. So marking a function with view tells web3.js that it only needs to query your local Ethereum node to run the function, and it doesn't actually have to create a transaction on the blockchain (which would need to be run on every single node, and cost gas).\

*Note: If a view function is called internally from another function in the same contract that is not a view function, it will still cost gas. This is because the other function creates a transaction on Ethereum, and will still need to be verified from every node. So view functions are only free when they're called externally.*

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

After deployed in Ethereum, smart contracts are **immutable**. If there's a flaw in your contract code, there's no way for you to patch it later. You would have to tell your users to start using a different smart contract address that has the fix.**setKittyContractAddress** function that lets us change this address in the future in case something happens to the CryptoKitties contract.\

**Ownable** means the external contracts have an owner who has special privileges to update and modify.\
1.*When a contract is created, its constructor sets the owner to msg.sender (the person who deployed it)*

2.*It adds an onlyOwner modifier, which can restrict access to certain functions to only the owner*

3.*It allows you to transfer the contract to a new owner*

**OpenZeppelin** is a library of secure and community-vetted smart contracts that you can use in your own DApps.\
**Constructor** is an optional special function that has the same name as the contract. It will get executed only one time, when the contract is first created.\
**Modifier** modifiers are kind of half-functions that are used to modify other functions, usually to check some requirements prior to execution.\
**OnlyOwner** can be used to limit access so only the owner of the contract can run this function. It's such a common requirement for contracts that most Solidity DApps start with a copy/paste of this Ownable contract, and then their first contract inherits from it.\

In Solidity, your users have to pay every time they execute a function on your DApp using a currency called **gas**. Users buy gas with Ether (the currency on Ethereum), so your users have to spend ETH in order to execute functions on your DApp. Each individual operation has a **gas cost** based roughly on how much computing resources will be required to perform that operation (e.g. writing to storage is much more expensive than adding two integers). The total gas cost of your function is the sum of the gas costs of all its individual operations. Because running functions costs real money for your users, code optimization is much more important in Ethereum than in other programming languages.\

The creators of Ethereum wanted to make sure someone couldn't clog up the network with an infinite loop, or hog all the network resources with really intensive computations. So they made it so transactions aren't free, and users have to pay for computation time as well as storage.\

Normally there's no benefit to using these sub-types because Solidity reserves 256 bits of storage regardless of the uint size. For example, using uint8 instead of uint (uint256) won't save you any gas. But there's an exception to this: **inside structs**. If you have multiple uints inside a struct, using a smaller-sized uint when possible will allow Solidity to pack these variables together to take up less storage. For this reason, inside a struct you'll want to use the **smallest integer sub-types** you can get away with.\

You'll also want to cluster identical data types together (i.e. put them next to each other in the struct) so that Solidity can minimize the required storage space. For example, a struct with fields uint c; uint32 a; uint32 b; will cost less gas than a struct with fields uint32 a; uint c; uint32 b; because the uint32 fields are **clustered together**.\

**Calldata** is somehow similar to memory, but it's only available to external functions.\

In order to keep costs down, you want to avoid writing data to storage except when absolutely necessary. Sometimes this involves seemingly inefficient programming logic — like rebuilding an array in memory every time a function is called instead of simply saving that array in a variable for quick lookups. In most programming languages, looping over large data sets is expensive. But in Solidity, this is way cheaper than using storage if it's in an external view function, since view functions don't cost your users any gas. \

Use a for loop to build the contents of an array in a function rather than simply saving that array to storage.\
