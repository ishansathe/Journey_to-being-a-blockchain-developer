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
