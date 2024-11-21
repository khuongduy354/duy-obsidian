```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}

-> anyone can get or change data from the blockchain
```
-> A Solidity **contract** is **_functions_** and **data** at a specific **address** on the **Ethereum** blockchain

# Coin examples 
```java
contract Coin {
   //create a minter variables getter function named minter 
    address public minter; 
 
    //balances is a hashmap 
    mapping (address => uint) public balances;
    
    constructor() {
        minter = msg.sender;
    }

// producing new coins, only creator can.
    function mint(address receiver, uint amount) public {
    // require is like revert without error info 
        require(msg.sender == minter);
        balances[receiver] += amount;
    }



//error type 
error InsufficientBalance(uint requested, uint available);
// event type to emit
event Sent(address from, address to, uint amount);
// send money, emit events to notify success transaction. 
    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
        // stop & abort all changes
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
//see Solidity docs for listening to event 
}
```
-> special global variable [Units and Globally Available Variables — Solidity 0.8.16 documentation](https://docs.soliditylang.org/en/v0.8.16/units-and-global-variables.html#special-variables-functions)
-> miniting see [[BlockChain#Concensus]]

# Persistent
[Storage vs. Memory vs. Stack in Solidity & Ethereum - DLT-Repo](https://dlt-repo.net/storage-vs-memory-vs-stack-in-solidity-ethereum/)
![[Pasted image 20220824174513.png]]
- Storage: store in blockchains (increase gas fee)
- Memory:  store during function execution
- Stack: code are pushed to stack for executions


# solidity
--- 
# Refererences 




2022 08 24 15:18
#cheatsheet [[solidity]] [[BlockChain]] [[smart contract]]