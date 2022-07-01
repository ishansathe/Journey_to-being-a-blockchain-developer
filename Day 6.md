Learnt about Payable functions
the values and relations of wei gwei and ether

wei 1000000000000000000

gwei 1000000000

ether 1

We don't get decimal value in the ethereum crypto, instead, we only see them in smaller denominations. Wei is the smallest denomination in the Ethereum Blockchain.
Payable functions are red in color

Learnt about mapping msg.sender and msg.value

     mapping (address => uint256) FundedValue;
     
Then, in the function
        we use
        
        function fund ()public payable
        {
                FundedValue [msg.sender] += msg.value;
        }
        
        Here, msg.sender is the person who called the function/contract and msg.value is the value set by the caller.
        
        As we deploy this code, we can see the function 'fund' in red color. This means the function is payable and can send and receive funds. Now, if we pass an value (which will be just below the gas limit viz below account that you selected to deploy the contract). Then that value will be sent to the contract and stored there. The existence and amount of funds will be stored inside the mapping. While the proof of transaction happening by any address can be seen from the transaction history!

In real world scenarios, we face the problems of funding such that, 
        The value of currency keeps changing
        we need to know the correct value when crypto is transferred so that the correct amount can be requested
        For example if 0.5 eth costs approximately 1Lakh INR then after few days or weeks the price could be more or less than 1 lakh (given volatility of crypto)
        
So, we need someone to bring these values to us
    Blockchains, being deterministic, can't have outside values. so we need an application or system that contacts with the outside world that returns correct values every time. This is done by help of Oracles. This is also where chainlink comes in.
    Now, A centralized (single) oracle is not enough as we went through all this trouble of having our blockchain decentralized, that now we don't want the outer entity to be decentralized. It's like putting our efforts to waste
    So, we go for decentralized oracles as well
    
    This is where chainlink comes in
    Not sure how its making these happen, but it offers us many conersion rates and we can take our pick. These are also called as DataFeeds
    
These are also currently being used by some of the protocols in the top defi systems like Synthetix is securing $2 Billion, sushi swap for leveraging trades, set protocol commodity mondey; Ave for understanding price of a collateral
This is an example of an out of box decentralized solutions that's already been packaged in the decentralized manner for you to consume and for you to use.This makes going to production a thousand times easier than building everything yourself

Now, let's see the code for getting datafeeds. We will be first looking at the default code interface provided to us by chainlink. You can see this here "https://docs.chain.link/docs/get-the-latest-price/" .
view the code and click on "view in remix" It will open the remix IDE with code on it.
     We will be deploying this in real time network so we are now using "InjectedWeb3" in the VM option. You will get a notification from metamask, just confirm it.
     Also, this will be performed in the Rinkeby Network, so switch to that network. (The guide suggested Kovan which doesn't work so I changed it. The chainlink documentation asks us to use Rinkeby network.)
     
     A thing to note is that, this test will take the currency of "Rinkeby Network". So you are suggested to get some free funds from Rinkeby Faucet "https://faucets.chain.link/"
 
     Now, let's deploy the contract, metamask is gonna pop up, we confirm that
     then, we click on getLatestPrice, to get the latest price!
     You might be asking "Why this number look so big?"Remember how we talked about wei, gwei and either.Well the reason that those exist is because decimals don't work in solidity.We actually have to return a value that's multiplied by 10 to some number.
     
     If we get a value of 102024614766, this value is actually 1020.24614766 *10^8.
     
     Next question you might wanna ask is why did we work with this on a testnet? Why can't we do this on a local network? The answer to this is because there're no Chainlink nodes on a simulated JavaScript VMs.We'll learn later how to actually mock these interactions and mock a chainlink node returning data onto our blockchain.But for now let's head back over to the contract that we're working on so we can learn how to implement this latestprice in any contract that we ever want to.
     
     
   ## Importing DataFeed code from Chainlink npm package
     How do we implement this data feed in our FundMe application?
     We import the interface.
     Now, a thing to be noted is that the version numbers change so the path also changes. Look at the path that follows in the sample workspace that you opened (after going to that chainlink doccumentation page). It will show you the path 
     
     As for me, this is my path
               import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
               
          What changed for me, is v0.6 -> v0.8 and Aggregatorv3Interface.sol -> AggregatorV3Interface.sol
          
          If you follow the import path, you will end up to the interface.
          The contracts don't start with a contract keyword but an interface keyword. The main difference you can see is that their functions aren't completed. They just have function name and its return type.
          In our code here, solidity doesn't natively understand how to interact with another contract. We tell solidity what functions can be called on another cntract. This is where interfaces come in
          
#### ABI 
     Interfaces actually compile down to ABI. It tells solidity what functions can be called on another contract. We need solidity o know what functions it can use and what functions it can call other contracts with.
     Anytime you're going to interact with another contract in solidity or smart contract programming in general, you're going to need that contracts ABI
     
### Interacting with an interface contract
     Interacting with an interface contract is done in the same way as interacting with a struct or variable
     Let's define a new function getVersion and we are going to call the version function of our interface onto our contract.
     The same way we define variables and structs, we define working with other contracts and interfaces.
     
    function getVersion() public view returns (uint256)
    {
        AggregatorV3Interface rate = AggregatorV3Interface (0x8A753747A1Fa494EC906cE90E9f37563A8AF630e)
    }
    
    First thing we named is time which is AVI. Since we're inside the contract, we are going to skip visibility and give the name "rate". Then we initialize the contract. How do we choose where to interact with the contract? We pass the address of where the contract is located. (will tell you more of it later don't fret now)
     

## Finding the PriceFeed Address
     In order to find where this Eth/USD is located on the rinkeby chain, we can look at the ethereum price feeds "https://docs.chain.link/docs/ethereum-addresses/" chainlink documentation
     It has a ton of pricefeeds and even more non pricefeed related data. Scroll to rinkeby because on each different chain, the contract address that has all the price information is going to be different. Scroll down and find ETH/USD
     Copy the address and pass it to AVI initialization (this is actually the address that we entered before - scroll up)

     It is saying that we have a contract that has a "AggregatorV3Interface" contract's function (defined in the interface) located at that address.
     If that's true, we should be able to call "rate.version".
     
## Deploying
     Let's compile it and deploy it to testnet(Injected Web3 as Environment).Remember that address is located on a actual testnet.On an actual network.We'll learn later on how we can actually work with a simulated chain and work with these price feeds but that's much later in the journey.
     
