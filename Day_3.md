Signing and Verifying transactions
  We have Public and Private Keys
    Public Keys are used to verify
    Private Keys are used to sign

Signatures
  Signatures are formed when we enter some data and hash it with our private key. For the same set of data, the same signature will be generated (from same sender)
  The key here is that you can generate signature from data and private key, but one cannot generate private key from data and signature
  People can later use your public key to verify whether that transaction is truly signed by you.
  
  Authenticity is ensured in blockchain with this, as if someone tries to fake a transaction under your name, then it will be easily found out when transaction gets verified

Note: If your private keys are stolen then people can sign transactions under your name and all your funds will be compromised. So keep them safe.

Concepts of blockchain remain the same across various blockchains:
  (That is, the hash and hash function will be the same even if hashing algorithms or implementation of blockchain changes)

Node:

  Node is a single entity on the blockchain network. It is one of the servers running the blockchain technology. All of these nodes running together form the blockchain technology.
  Also, once hardware requirement is fulfilled, one can go to github, download and install the software and start running their own ethereum node.
  
Centralized vs Decentralized
  
  Centralized:
    In Centralized networks, the network is controlled by a single entity or organization(group of entities) who hold the power to change data, modify functionality and shut down
    the system whenever they feel like.
    Say these entities go down, then the whole network goes down and any valuable information may also be lost.
    An attacker has to just manipulate the data within that entities servers to get control. This can be avoided with help of high level secirity systems or setting the risk to be higher than the reward.
    One Advantage that they provide is high speed transactions which decentralized systems can't exceed (unless its a very poorly built centralized system).
    
  Decentralized:
    In Decentralized Networks, the power is with the community. One cannot simply modify the workings or change the data on the network.
    If a single node goes down, or even all except one node goes down, then too the whole network will continue to run as long as that single node is running. Any transactions made during that time period will also be recorded.
    Modifications of data on just 1 server is not a threat as the malicious or incorrect node (after identification by checking if in sync with other nodes) will simply be ignored, removed, etc. 
    One disadvantage is that the more the nodes are included in the validation or consensus part, the slower the transactions will be ( this is just 1 among many other reasons)
    Also, Transactions and interactions are listed on the blockchain. so if anyone tries to change some things, then their hashes are going to be out of syn. This gives blockchain the immutability feature.

Consensus:
  Consensus is achieved when all the nodes in the network agree on a particular block. The consensus where we simply check for each others ledgers and hashes is the most basic form of consensus called Paxon Method.
  There are many forms of Consensus mechanisms which perform against different problems and are based on different ideas
  Some are listed
    BFT(Byzantine Fault Tolerance) Based Consensus
    Nakamoto Consensus Protocols
      Proof of Work
      Proof of Stake
      Proof of Authority
      Proof of Activity
      Proof os Space
      Proof of Capacity
      Proof of Burn
    GHOST(Greedy Heaviest Observed Sub-Tree) protocol
    Then we have Correct By Construction Protocols (which may work under the aforementioned protocols) used to face specific problems only like the Casper protocol (Friendly GHOST version which works only in Proof of Stake).
    
  Now Let's look at 2 of problems/attacks
    a) Sybil Attacks
      Sybil Attacks occur when an individual (or organisational entity) attempts to take over the network or gain benefits of block rewards & transaction fees by creating multiple pseudonymous accounts to increase odds of winnings.
      This is avoided in Proof Of Work. The amount of computational power required to mine the nonce (that is the closest correct value of hash) for the block to be added onto the blockchain is very high. Moreover, the node has to be the first to find out the correct value. So attacker has less incentive to perform this attack.
      Another thing to note in PoW is that you can also increase/decrease the difficulty of the problem, thus reducing or increasing block time.
      
      This is dealt with in Proof of Stake too. When Validating (a concept in PoS where block is validated instead of mined), the validators have to stake some of their own funds to gain voting rights. So creating multiple pseudonymous accounts is not very smart as attacker has to stake their own funds there as well which may in turn get slashed again (for malicious behaviour). 
      Besides, attacker doesn't want to harm the blockchain in which they have invested so much.
      
    b) Forking
      Another problem that occurs is that in a blockchain, sometimes 2 blocks are added at the same time, but not on top of each other. This causes forking. Now people start building the chain over the blocks that they believe in. In this case, to solve the ambiguity or to determine which block to consider as the core chain, we see which
      fork is the longest. The one which is the longest chain (that is, has most blocks mined in- after the fork happened) is considered as the main chain. The protocol used to achieve this is called the GHOST(Greedy Heaviest Observed Sub-Tree) Protocol.
      
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
