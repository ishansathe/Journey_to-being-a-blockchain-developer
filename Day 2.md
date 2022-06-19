Created the metamask account, got some fake eth (Rinkeby Testnet) and checked its validity on etherscan.io (it occurs on a different chain- you will be prompted)
Learnt about Mainnets and Testnets
Etherscan is a block explorer, it helps us find out things that happen in a blockchain easily
  Learnt about the variables and constants in transactions
  Learnt about Gas,
    The basic computation power reqquired/expended to include a transaction into the blockchain. THe amount of gas requires depends on the amount of energy required by 
    block chain to conduct that transacton. Ex: Running a single transaction may cost 21000 gas. However, running a transaction that calls a smart contract and does
    a bunch of other stuff will cost much more gas.
  Gas Price
    How much eth/native token it costs per gas
  Transaction Fees
    Gas Used * Gas Price
  Gas Limits
    Maximum Amount of Gas in a transaction. You may wonder, why pay more than required amount? That's because there are so many transactions that are to be added onto
    the blockchain that the miners want to mine those transactions that will give them more returns(money).
  We also find out about Gas Estimator from gasstation.info
  While Transacting with Metamask we can look into all these things. Typically, we can set the gas price based on people that are working on the blockchain. If there are
  a lot of people attempting to go into the blockchain then the price will be high but if there are very few people transactiing on the blockchain then the price will be 
  low (It's sort of intuitive)
       
       
       
Okay so next we learnt about
Hashing
  Blockchain uses SHA256 that has a diferent value for each input. However, the same input will give the same value no matter how many times it is entered. For ex. Let's
  say we have an empty data set. Then we get the hash for it. Now we add some value to it(say the word 'blockchain'), the hash changes.
  In Blockchain, we use Nonce(Number used once) value to help find the hash value.
  How do we even get the hash value?
       In Blockchain, To receive the hash value, one has to mine for it. It goes from 1 to whichever number of nonce that gives hash value starting with four 0s. This process is called mining. So this satisifies the little definition of what a signed block is.
  Now, To connect these blocks. The next block has the hash of previous block. The Genesis block which is the first block has hash of 0s as an imaginary block. As there
  are no blocks before it.
  Now, the value of blocks can be changed. Let's see what happens. Assume we have 5 blocks each connected well with proper hashes. I go back in history and change the
  value of block 3. Now, the hash value changes for block 3 and all the blocks from block 3 are also invalid. So, I now have to mine again from block 3 till the current
  block 5. This takes a lot of time
  This condition is solved even further by blockchain. So, Blockchain is a distributed ledger system. Each peer has a copy of the blockchain. So let's say Person B 
  changes the block 3 and manages to hash till block 5. Now the block 5 begins with has 0xacf... However, in the copy of Person A and Person C, block 5 begins with 
  0xef8... Thus block 0xef8 wins (majority) and is considered as valid. This enforces the immutability. Moreover, one need not go back till the block that was changed. 
  Simply checking the current block is enough as any previous change will cause a chain of changes till the current block which will be easily noticeable.
       
Now, let's look at Tokens and Transaction history
       Tokens are just essentially transformations of data into.. smaller formats. Tokens can be the same  as in fungible tokens or non fungible tokens. Difference
       between a cryptocurrency and a token is that a cryptocurrency token say eth is native to ethereum. However there are also other tokens like DAI, LINK, COMP, etc
       That can serve a multitude of functions on the platforms on which they are built like DeFi, Games and accessing services.
       There also exist Non Fungible Tokens such that there exists only 1 kind of that token.
       
       There can exist problems of ownership of tokens or whether even if the token came out of thin air. In the following example I am considering the native 
       cryptocurrency for Ethereum. The same can be applicable to other forms of tokens as well.
    This thing is solved by using Coinbase
       In CoinBase, Let's say Alex receives 3 eth from Alice. Where did Alice receive these tokens from? Thin air?
       We can go back in history and we see that Alice received 5 eth from Bob and that Bob received 10 eth through a Coinbase Transaction(in the Genesis Block).
       Now we know that all the funds that are being transferred dont go beyond the ones that htey had and this follows the basic rule of a currency that you cant 
       invent it out of thin air. It's dispersion is controlled
       And hence, we can say that the transaction is valid. As Bob infact was in possession of these tokens. (He may have bought these in exchange for local currency).
       
