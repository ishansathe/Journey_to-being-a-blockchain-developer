## SafeMath and Integer Flow!

  Let's talk about pitfalls of solidity when it came to math
  Prior to 0.8, if you added to the max size, a uint number could be wrapped around the lowest number that it would be. 
  Ex: if we add 2 uint8 number: 
    255 + uint8(1) = 0 
    255 + uint(100) = 99
    
  This is because integers can wrap around when they reach their maximum cap. They basically reset.
  This is something we need to watch out for when working with solidity. If we're doing multiplication on really big numbers, we can accidentally pass this cap. Luckily  as a version of solidity 0.8, it actually checks for overflow (as a default) to increase readability of code even if it slightly increases gas costs.
  Just remember that if using a verison lower than 0.8 then you will have to do something about this.
  
  We could write a code to check all of our math or we could just import Safe Math from another package. Similar to 'chainlink', we can import safemath from the openzeppelin tool
  
  Open zeppelin is a tool that allows us to import a lot of prebuilt contracts.
  
      import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v2.5.0/contracts/math/SafeMath.sol";
      
    Now I have imported from the github link that was presented on the remix documentation. In that, I had to change the solidity version of the SafeMath file ( I have currently set it to ^0.8.0; and also set the license Identifier to MIT. Then, both of the compilations (for the safemath and my original contract) have been successful.

## Libraries 
  Libraries are similar to contracts just that they are only deployed once at a specific address and their code is set to be reused.
  Using keyword: The directive using A for B; can be used to attach library functions (from the library A) to any type (B) in the context of a contract.

In this case we're attaching a SafeMath chainlink library to uint256 so that these overflows are automatically checked for.

This is for those of you who are familier with SafeMath and integer overflows and underflows.We're not going to be calling the functions that SafeMath provides us like div, add, mull all those functions.Simply because in 0.8 moving forward we no longer have to use those.We can just use regular operator like '+' & '-'.

## Setting Threshold
  We have a way to get the conversion rate of whatever eth is sent to us and turn it into USD. Now, we set a threshold to make sure that whatever value that we set (30$) has to be minimum amount sent by the users
  First, we set minimum value by using 
      
        uint256 minUSD = 30 * 10 ** 18;
        
  Now that we have a minimum amount, how do we make sure that amount sent to us is same or more than amount mentioned?
#### Require statement
  
    require( convert (msg.value) >= minUSD );     //Convert function is one we have defined and declared previously.
    
 When a function call reaches a require statement, it checks for truthfulness of any require that you have asked for. In our case, the converted value of msg.value has to be greater than or equal to 50$. (We don't concern ourselves with receiving value more than 50$ cuz why won't we want more money?). If not, we we will revert the transaction.
 *Revert* If the converted rate of msg.value is less than 50$, we're going to stop executing.We're going to kick it out and revert the transactions.This means user gonna get their money back as well as any unspent gas and this is highly recommended.We can also add a revert error message.
 require(convert(msg.value) >= minUSD, "You need to spend more eth");
 
 
### Deploying the transaction.
    After deploying if we still try to submit less than min value. Then we get an error message.
    The contract isn't even letting us to make the transaction.Whenever you see gas estimation failed errors usually that means something reverted or you didn't do something that was required.
    
    Remember, you have to be using the Injected Web3 
    
      If you enter value lesser than 30 $ (as we have set it) then you will get the message
            
            Gas estimation errored with the following message (see below). The transaction execution will likely fail. Do you want to force sending?
execution reverted: You need to spend more eth { "originalError": { "code": 3, "data": "0x08c379a00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000001a596f75206e65656420746f207370656e64206d6f726520657468000000000000", "message": "execution reverted: You need to spend more eth" } }

### Withdraw Function
    We can fund this contract now with certain minimum USD value. You may have noticed that we  are funding this contract but not doing anything with those funds. We don't have a function to withdraw the funds that are funded into the contract. There's no way to get it back. How do we fix this?
    
        function withdraw() payable public {    }
        
    The above function will also be payable because we are transferring ETH.
    
 ### Transfer, Balance, this
        
        Transfer is a function we can call on any address to send eth from on address to another. In this case, we are transferring eth to the msg.sender. We will be sending all the money that is funded. So in order to get all the money that has been funded, we did 
        
                msg.sender.transfer(address(this).balance);
        
        'this' is a keyword in solidity. Whenever you refer to 'this', we are talking about the contract we are in and when we say address(this) then it means this contract's address.
         Whenever we call an address and then the balance attribute, you can see the balance in ether of the contract.
         So with that line we are saying whoever called the function will be sent all the eth inside the contract because the caller will be msg.sender to that function.
         
### Deploying
    Now, if we deploy the contract and fund it with 1 eth, this amount will be deducted from that address and be sent to the contract. Then calling the withdraw function (without changing the account) will allow us to retrieve the funded amount!
  
