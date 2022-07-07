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
      
        uint256 minUSD = 30;
        
        (the value entered in the reference code by spo0ds is absurd.)
        
  Now that we have a minimum amount, how do we make sure that amount sent to us is same or more than amount mentioned?
  
#### Require statement
  
    require( convert (msg.value) >= minUSD );     //Convert function is one we have defined and declared previously.
    
 When a function call reaches a require statement, it checks for truthfulness of any require that you have asked for. In our case, the converted value of msg.value has to be greater than or equal to 30$. (We don't concern ourselves with receiving value more than 30$ cuz why won't we want more money?). If not, we we will revert the transaction.
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
         
         [A thing to note is that remix may send you the message (when you compile) that you cannot convert type address to address payable implicitly. In order to deal with that problem, I came up with the following code:
              
              address payable a = payable(msg.sender);
              a.transfer(address(this).balance);
              
         Entering this should solve the problem. What's happening here is I'm explicitly converting msg.sender to payable (it is non payable by default)]
         
### Deploying
    Now, if we deploy the contract and fund it with 1 eth, this amount will be deducted from that address and be sent to the contract. Then calling the withdraw function (without changing the account) will allow us to retrieve the funded amount!
    
## Owner, Constructor
    Now, we don't want *anyone* to be able to withdraw funds from the contract. We wish to allow a selective individual. Say just the owner of the contract.
    We said before that require function can take care of certain conditions to be met.
    So, we use that
        require ( msg.sender == owner);
    However, we don't have an owner to this contract yet. How do we set an owner the moment it is deployed?
    We could create a function onlyOwner. But the owner would change the moment this function would get deployed.
    So here, we make use of the constructor! It calls the function the moment the smart contract is deployed. So typically, at top of your code you would see a constructor that is a function that gets called the instant a smart contract is deployed.
    
    constructor()
    {
        owner = msg.sender;
        //the sender of message will be the owner of this contract (i.e us, in this case)
    }
    
    A thing to note is that unlike in Java or other langauges you may have learnt, these constructors need not be named the same as the contract they belong in. Just label them as constructor and you're good!
    
    Along with this, we add in the **withdraw() function**
        require (msg.sender == owner);
        //Making sure the withdrawer is the initiator of this contract and not some greedy boii.
    
    and then deploy
    
    Now, as you call the withdraw function. The transaction will only be successful if you call it from the same account from which you deployed the contract. Else, it will fail.
    

## Modifiers
      We have set it so, that only the owner qill be able to call the withdraw function. But what if there are a ton of *contracts* that want to use this require(owner == msg.sender)?
      Is there an easier way to wrap our functions and some require or other executables?
      
      This is where modifiers come in. We can use modifiers to write in the definition of our function. Add some parameters that allows it only to be called by our admin contract. Modifiers are used to change the behaviour of a function in a declarative way. Let's create our first modifier
      
      modifier admin{
          require (owner == msg.sender);
          _;
      }
      
      What a modifier does, is before we run the function, execute the require statement and then from where the underscore (_) lies, run the rest of the function's code.
      Now, what we do is make the withdraw function as admin. What will now happen is before we do the transfer, it's going to check the modifier which runs msg.sender == owner
      So the code becomes like this
      
     modifier admin
    {
        require (msg.sender == owner);
        _;
    }

    function withdraw() admin public payable
    {   
        address payable a = payable(msg.sender);
        a.transfer(address(this).balance);
    }
  
### Resetting the Funder's balances to 0
    Now we have done everything except 1 thing. After we withdraw the funds. The balances of funders in the contract remain the same. We have to clear them now. We will have to go through all the funders in this mapping and reset their balances to 0 but how do we do that?

##### For Loop
    We can use the for loop to iterate through all the keys in the mapping. When a mapping is initialized, every single key is initialized. We obviosuly can't go through every single key on the planet, however, we can indeed create another data structure called an 'Array'!
    Let's go and create a funder's array in a way that we can loop through them and reset everyone's balances to zero.
