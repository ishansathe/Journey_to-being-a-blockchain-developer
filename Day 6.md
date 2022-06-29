Learnt about Payable functions
the values and relations of wei gwei and ether
wei 1000000000000000000
gwei 1000000000
ehter 1
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
