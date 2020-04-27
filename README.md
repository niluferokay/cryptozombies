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

**Payable** functions are a special type of function that can receive Ether.In Ethereum, because both the money (Ether), the data (transaction payload), and the contract code itself all live on Ethereum, it's possible for you to call a function and pay money to the contract at the same time.\
**msg.value** is a way to see how much Ether was sent to the contract, and **ether** is a built-in unit.

*Assuming `OnlineStore` points to your contract on Ethereum:*\
*OnlineStore.buySomething({from: web3.eth.defaultAccount, value: web3.utils.toWei(0.001)})*

Notice the **value** field, where the javascript function call specifies how much ether to send (0.001). If you think of the transaction like an envelope, and the parameters you send to the function call are the contents of the letter you put inside, then adding a value is like putting cash inside the envelope — the letter and the money get delivered together to the recipient.

And most important a variable to send and transfer ether instruction, it have to be a **address payable** type. Casting any integer type like **uint160** to address produces an address payable.

You can transfer Ether to an address using the **transfer** function, and **address(this).balance** will return the total balance stored on the contract. So if 100 users had paid 1 Ether to our contract, address(this).balance would equal 100 Ether.

You can use transfer to send funds to any Ethereum address. For example, you could have a function that transfers Ether back to the **msg.sender** if they overpaid for an item:

uint itemFee = 0.001 ether;
msg.sender.transfer(msg.value - itemFee);

Or in a contract with a buyer and a seller, you could save the seller's address in storage, then when someone purchases his item, transfer him the fee paid by the buyer: **seller.transfer(msg.value)**.

How can I securely generate a random number in my smart contract?
https://ethereum.stackexchange.com/questions/191/how-can-i-securely-generate-a-random-number-in-my-smart-contract

A **token** is a contract that keeps track of who owns how much of that token, and some functions so those users can transfer their tokens to other addresses.\

Now we're going to continue our ERC721 implementation by looking at transfering ownership from one person to another.

Note that the ERC721 spec has 2 different ways to transfer tokens:

*function transferFrom(address _from, address _to, uint256 _tokenId) external payable;*

and

*function approve(address _approved, uint256 _tokenId) external payable;*

*function transferFrom(address _from, address _to, uint256 _tokenId) external payable;*

*The first way is the token's owner calls transferFrom with his address as the _from parameter, the address he wants to transfer to as the _to paramater, and the _tokenId of the token he wants to transfer.*

The second way is the token's owner first calls approve with the address he wants to transfer to, and the *_tokenID* . The contract then stores who is approved to take a token, usually in a mapping (uint256 => address). Then, when the owner or the approved address calls transferFrom, the contract checks if that msg.sender is the owner or is approved by the owner to take the token, and if so it transfers the token to him.

Let's say we have a uint8, which can only have 8 bits. That means the largest number we can store is binary 11111111 (or in decimal, 2^8 - 1 = 255).

Take a look at the following code. What is number equal to at the end?

uint8 number = 255;
number++;
In this case, we've caused it to overflow — so number is counterintuitively now equal to 0 even though we increased it. (If you add 1 to binary 11111111, it resets back to 00000000, like a clock going from 23:59 to 00:00).

An underflow is similar, where if you subtract 1 from a uint8 that equals 0, it will now equal 255 (because uints are unsigned, and cannot be negative).

While we're not using uint8 here, and it seems unlikely that a uint256 will overflow when incrementing by 1 each time (2^256 is a really big number), it's still good to put protections in our contract so that our DApp never has unexpected behavior in the future.

A library is a special type of contract in Solidity. One of the things it is useful for is to attach functions to native data types.

For example, with the SafeMath library, we'll use the syntax using SafeMath for uint256. The SafeMath library has 4 functions — add, sub, mul, and div.

assert is similar to require, where it will throw an error if false. The difference between assert and require is that require will refund the user the rest of their gas when a function fails, whereas assert will not. So most of the time you want to use require in your code; assert is typically used when something has gone horribly wrong with the code (like a uint overflow).

*To prevent overflows and underflows, we can look for places in our code where we use +, -, *, or /, and replace them with add, sub, mul, div.*

The standard in the Solidity community is to use a format called natspec, which looks like this:

/// @title A contract for basic math operations
/// @author H4XF13LD MORRIS 💯💯😎💯💯
/// @notice For now, this contract just adds a multiply function
contract Math {
  /// @notice Multiplies 2 numbers together
  /// @param x the first uint.
  /// @param y the second uint.
  /// @return z the product of (x * y)
  /// @dev This function does not currently check for overflows
  function multiply(uint x, uint y) returns (uint z) {
    // This is just a normal comment, and won't get picked up by natspec
    z = x * y;
  }
}

@title and @author are straightforward.

@notice explains to a user what the contract / function does. @dev is for explaining extra details to developers.

@param and @return are for describing what each parameter and return value of a function are for.

Note that you don't always have to use all of these tags for every function — all tags are optional. But at the very least, leave a @dev note explaining what each function does.

the Ethereum network is made up of nodes, with each containing a copy of the blockchain. When you want to call a function on a smart contract, you need to query one of these nodes and tell it:

1. The address of the smart contract
2. The function you want to call, and
3. The variables you want to pass to that function.
Ethereum nodes only speak a language called JSON-RPC, which isn't very human-readable.Luckily, Web3.js hides these nasty queries below the surface, so you only need to interact with a convenient and easily readable JavaScript interface.
Or you can simply download the minified .js file from github and include it in your project: *<script language="javascript" type="text/javascript" src="web3.min.js"></script>*

##Web3 Providers

Now that we have Web3.js in our project, let's get it initialized and talking to the blockchain.

The first thing we need is a Web3 Provider.

Remember, Ethereum is made up of nodes that all share a copy of the same data. Setting a Web3 Provider in Web3.js tells our code which node we should be talking to handle our reads and writes. It's kind of like setting the URL of the remote web server for your API calls in a traditional web app.

You could host your own Ethereum node as a provider. However, there's a third-party service that makes your life easier so you don't need to maintain your own Ethereum node in order to provide a DApp for your users — Infura.

Infura is a service that maintains a set of Ethereum nodes with a caching layer for fast reads, which you can access for free through their API. Using Infura as a provider, you can reliably send and receive messages to/from the Ethereum blockchain without needing to set up and maintain your own node.

You can set up Web3 to use Infura as your web3 provider as follows:

*var web3 = new Web3(new Web3.providers.WebsocketProvider("wss://mainnet.infura.io/ws"));*

However, since our DApp is going to be used by many users — and these users are going to WRITE to the blockchain and not just read from it — we'll need a way for these users to sign transactions with their private key.Metamask is a browser extension for Chrome and Firefox that lets users securely manage their Ethereum accounts and private keys, and use these accounts to interact with websites that are using Web3.js.
And as a developer, if you want users to interact with your DApp through a website in their web browser (like we're doing with our CryptoZombies game), you'll definitely want to make it Metamask-compatible.

##Talking to Contracts

Now that we've initialized Web3.js with MetaMask's Web3 provider, let's set it up to talk to our smart contract.

Web3.js will need 2 things to talk to your contract: its address and its ABI.

Contract Address
After you finish writing your smart contract, you will compile it and deploy it to Ethereum. We're going to cover deployment in the next lesson, but since that's quite a different process from writing code, we've decided to go out of order and cover Web3.js first.

After you deploy your contract, it gets a fixed address on Ethereum where it will live forever. If you recall from Lesson 2, the address of the CryptoKitties contract on Ethereum mainnet is 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d.

You'll need to copy this address after deploying in order to talk to your smart contract.

Contract ABI
The other thing Web3.js will need to talk to your contract is its ABI.

ABI stands for Application Binary Interface. Basically it's a representation of your contracts' methods in JSON format that tells Web3.js how to format function calls in a way your contract will understand.

When you compile your contract to deploy to Ethereum (which we'll cover in Lesson 7), the Solidity compiler will give you the ABI, so you'll need to copy and save this in addition to the contract address.

Since we haven't covered deployment yet, for this lesson we've compiled the ABI for you and put it in a file named cryptozombies_abi.js, stored in variable called cryptoZombiesABI.

If we include cryptozombies_abi.js in our project, we'll be able to access the CryptoZombies ABI using that variable.

Instantiating a Web3.js Contract
Once you have your contract's address and ABI, you can instantiate it in Web3 as follows:

// Instantiate myContract
var myContract = new web3js.eth.Contract(myABI, myContractAddress);

##Calling Contract Functions

Our contract is all set up! Now we can use Web3.js to talk to it.

Web3.js has two methods we will use to call functions on our contract: call and send.

Call
call is used for view and pure functions. It only runs on the local node, and won't create a transaction on the blockchain.

Review: view and pure functions are read-only and don't change state on the blockchain. They also don't cost any gas, and the user won't be prompted to sign a transaction with MetaMask.

Using Web3.js, you would call a function named myMethod with the parameter 123 as follows:

myContract.methods.myMethod(123).call()
Send
send will create a transaction and change data on the blockchain. You'll need to use send for any functions that aren't view or pure.

Note: sending a transaction will require the user to pay gas, and will pop up their Metamask to prompt them to sign a transaction. When we use Metamask as our web3 provider, this all happens automatically when we call send(), and we don't need to do anything special in our code. Pretty cool!

Using Web3.js, you would send a transaction calling a function named myMethod with the parameter 123 as follows:

myContract.methods.myMethod(123).send()
The syntax is almost identical to call().

###Getting Zombie Data
Now let's look at a real example of using call to access data on our contract.

Recall that we made our array of zombies public:

Zombie[] public zombies;
In Solidity, when you declare a variable public, it automatically creates a public "getter" function with the same name. So if you wanted to look up the zombie with id 15, you would call it as if it were a function: zombies(15).

Here's how we would write a JavaScript function in our front-end that would take a zombie id, query our contract for that zombie, and return the result:

Note: All the code examples we're using in this lesson are using version 1.0 of Web3.js, which uses promises instead of callbacks. Many other tutorials you'll see online are using an older version of Web3.js. The syntax changed a lot with version 1.0, so if you're copying code from other tutorials, make sure they're using the same version as you!

function getZombieDetails(id) {
  return cryptoZombies.methods.zombies(id).call()
}

// Call the function and do something with the result:
getZombieDetails(15)
.then(function(result) {
  console.log("Zombie 15: " + JSON.stringify(result));
});
Let's walk through what's happening here.

cryptoZombies.methods.zombies(id).call() will communicate with the Web3 provider node and tell it to return the zombie with index id from Zombie[] public zombies on our contract.

Note that this is asynchronous, like an API call to an external server. So Web3 returns a promise here. (If you're not familiar with JavaScript promises... Time to do some additional homework before continuing!)

Once the promise resolves (which means we got an answer back from the web3 provider), our example code continues with the then statement, which logs result to the console.

result will be a javascript object that looks like this:

{
  "name": "H4XF13LD MORRIS'S COOLER OLDER BROTHER",
  "dna": "1337133713371337",
  "level": "9999",
  "readyTime": "1522498671",
  "winCount": "999999999",
  "lossCount": "0" // Obviously.
}
We could then have some front-end logic to parse this object and display it in a meaningful way on the front-end.


Ask Question
EN
Chapter 5: Metamask & Accounts
Awesome! You've successfully written front-end code to interact with your first smart contract.

Now let's put some pieces together — let's say we want our app's homepage to display a user's entire zombie army.

Obviously we'd first need to use our function getZombiesByOwner(owner) to look up all the IDs of zombies the current user owns.

But our Solidity contract is expecting owner to be a Solidity address. How can we know the address of the user using our app?

Getting the user's account in MetaMask
MetaMask allows the user to manage multiple accounts in their extension.

We can see which account is currently active on the injected web3 variable via:

var userAccount = web3.eth.accounts[0]
Because the user can switch the active account at any time in MetaMask, our app needs to monitor this variable to see if it has changed and update the UI accordingly. For example, if the user's homepage displays their zombie army, when they change their account in MetaMask, we'll want to update the page to show the zombie army for the new account they've selected.

We can do that with a setInterval loop as follows:

var accountInterval = setInterval(function() {
  // Check if account has changed
  if (web3.eth.accounts[0] !== userAccount) {
    userAccount = web3.eth.accounts[0];
    // Call some function to update the UI with the new account
    updateInterface();
  }
}, 100);
What this does is check every 100 milliseconds to see if userAccount is still equal web3.eth.accounts[0] (i.e. does the user still have that account active). If not, it reassigns userAccount to the currently active account, and calls a function to update the display.
