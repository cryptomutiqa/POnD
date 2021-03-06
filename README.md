PonD(Proof of network Dispersity) BlockChain Project

Project Introduction
This project is aimed to build a set of blockchains with a new consensus protocol: Proof of network Dispersity, which could solve some common problems of existing POW and POS protocol to make it more environment friendly and fairer. On top of that, a series of blockchains with different tasks and functions will be established, necessary rewards will be distributed to developers within the protocol layer and simplify the business model of side-chain projects, which eventually makes this project as an open scalable system, and provides a safer, fairer and more flexible platform in the blockchain, especially cryptocurrency industry.

Proof of network Dispersity
(Patent NO. CN2018102289423)

Design Background
My initial design intention is to find a proof of capability by making use of the network latency to realize POW-like consensus with low energy consumption.

The “Proof of work” protocol is being criticized because of the energy consumption. But regardless of the energy problem, POW is still the most essential and effective method of the blockchain system. With the logic of “able people should get more pay”, proof-of-work is the first consensus protocol that makes a total decentralized system work. All of the other protocols are following the same logic, like Proof of Activity, Proof of Burn, Proof of Storage, Proof of Elasped Time, and so on. “Proof of stake” is the most popular one of them because it doesn’t need any external resources and consumes low energy by expanding the main logic from “able people should get more pay” to “rich people should get more pay”, which makes the mining competition is only among the static inner states and there is no need for power consumption. But “POS” mode has its own flaw such as “nothing at stake” which can casue multiple voting problem, “stake grinding attack” and so on. An inevitable problem of “POS” is caused by the logic: “rich people should get more pay”. Whenever the miners spend the same amount of time, the richer ones earn more, so that the rich miners tend to spend more time to work than others. In that case, the rich miners will gradually become richer and richer and this process will be ever-accelerated until there are only some richest users left in the system. By the way, some POS protocols offer “interests” to every stake holders to replace the mining process. It seems that they resolved the problem above but those protocols are vulnerable for lack of incentive to maintain the network.

The original intention of this article is to find another low energy consumption "proof of ability" to replace "proof of stake". Considering the following fact: Because of the network lantency, the more online terminals work together, the faster they pick up randomly distributed information in the network. And this is a kind of "ability" that I called “network dispersity”. Which can not be improved by enhancing the performance of a single machine or using more electric power. In the process of the designing, I realized that the "stake" property is inextricable and the best choice is to use both "ability" and "stake" at the same time and users could determine by themselves whether to use "stake" or "ability". By Guiding the users with appropriate profit distribution to ensure the most fairness and safety.

Benifits
Compared with "POW" and "POS", this model has the following benefits: 

No hashpower competition and no high-energy-consumption; ->
No "nothing at stake" and "stake grinding" attack;->
Optimize wealth distribution logic based on "stake" and replace all the inactive nodes with miners to maintain the network;->
The new way of mining competition helps change the environment of App or website development and improve the user experience.->
(note: Because this article is only discussing the situation of fully decentralized systems, protocols like "PBFT", "DPOS", "Ripple" will not be involved.)


Scenarios and Characters
The whole network can be imagined as kind of scenario of canvassing. There are 3 types of characters according to the node's functions.

Voter
Each node that broadcasts a transaction could be a Voter which is mainly responsible for answering the request of Workers and voting for the main chain. The stake that a Voter holds is seen as “votes” in this scene and the number of "votes" will influence miner's competition.Any user could be a Voter.

Miner
Miners have their own Workers canvassing “votes” for them throughout the network and use those votes to compete for generating blocks. Any user could be a Miner.

Worker
Workers work for the Miners to detect the Voters nearby and send a “canvass” request as soon as possible. Workers are mostly running on the network terminals by being embedded into the client Apps or web sources.


The network scheme is shown as below:
Alt text
Figure 1: nodes & network scheme illustration

Consensus Process
Before publishing a transaction, a Voter will broadcast a signal to preannounce it. The Workers nearby send “canvass” requests back when they detect the signal.
After receiving the first request from a Worker, the Voter packs the main chain’s (about selecting the main chain, refer to the GHOST protocol) tail-block “b” and the address “m” of the Miner who owns the Worker into the transaction structure , and then broadcasts the transaction.
(note：We need to add those two 32-byte fields "b" and "m" to the transaction field. And a "balance" field is needed too if other place does not store stakes of the account.)
This transaction is validated and packed into a new block by another miner, and then broadcasted in the network.
When a Miner receives a new block, he begins to validate it. At the same time, the Miner checks the field “b” and “m” of each transaction. Mark the transactions with “x” when their “m” fields point at the Miner himself and mark them with “y” when their “b” fields match the current chain’s tail-block.
Equally divide the stakes of each Voter between all of its transactions in that block.
Add up the balances divided at the previous step of all the transactions marked with both “x” and “y” until the Miner meets the target of generating a block. Denot the result by “X” and the max number of statisticed blocks is 100.
(note: The "x" mark means the vote of generating blocks, but why we also need to check the "y" mark, refer to Chapter "Frauds and attacks Article 1." )
Add up the divided balances of the transactions marked with “y” only in the current block and denote the result by “Y”
(note: The "y" mark means the vote of the main chain and there will be some adjustment about "Y", refer to Chapter "Frauds and attacks Article 2." )
The Miner is trying periodically to meet the target of generating a block with a mathematical operation based on constants such as timestamps and private signatures. That is denoted by: proofFunction() < target*d*X*Y, “d” means the difficulty adjustment parameter. Other blocks received during that process, competing for the main chain, should be parallel processed. In order not to weaken the main chain, dthere is no need to stop competing for the current chain if the weight of main chain is less than the existing chain plus eight, and it actually follows its own benefits (possible to main chain)
(note: The trying frequency should be floating, refer to Chapter "Frauds and attacks Article 1." )
After meeting the target of generating a block, the Miner packs all of the transactions received during this period into a new block and broadcasts it. All of the “xy” marked transactions (coded within 100 blocks in order to save some space), profits of all nodes and other parameters should also be packed for verification. Mining rewards will be distributed proportionally between miners and all the Voters marked with xy.
(note: Profits distribution strategy is discripted in Chapter "Reward distribution")
(note2: in terms of the extra storage space for validation parameters and reward distribution, it could be 2 more bites in the ID of each transaction and 4 more bites in the profit at maximum, which is fully acceptable; additional work of block validation is only going through the last 100 blocks, and it will not cause too much computation with proper algorithm.)

For better understanding, the steps can be briefed as:
  "Every time they publish a transaction, the stakeholders vote with their stake on the miners to generate a block and on the branches to be accepted as part of the main chain. The more stake a block or a miner get, the higher chance they win the competition."
note1: to control the voting frequency, it is necessary to count the block gap when the last transaction arrives at current block in each account, and take a certain granularity, such as 10 blocks, as the adjustment coefficient. From zero, every unit time, the coefficient increases proportionally, and only after a long interval, like 6000 blocks, around 100 hours, coefficient can be restored to the maximum; also, the same adjustment coefficient will be added to each UTXO, and transaction frequency can adjust the number of stakes with the same approach and parameters as the former.)
(note2: the introduction of voting time granularity is to restrict the voting frequency, reduce relevant transactions when generating blocks, and increase validation rate of blocks and transactions.)
(note 3: to improve efficiency, we can add transactions with pure voting, and users are able to decide whether they want to participate in the voting for the current transaction within the range of voting frequency.)

The following diagram shows the mining competition based on the network lanency:
Alt text
Figure 2: Process of competition for the Voters
As shown in the scheme above, the more online user an App has, the higher possibility it has of winning the votes(stakes). Which will help the miner win the mining competition later.


The record of those votes will be packed in blocks. The following diagrams show the principle about competition of block generation (x vote) and main chain (y vote) respectively:
Alt text
Figure 3: Validation of block generation votes
As shown in the scheme above, the system will count votes in certain existing blocks and the Miner with maximum votes can have the highest chance to win the competition.


Alt text
Figure 4: Validation of main chain votes
As shown in the scheme above, whenever there is any fork, the transactions will also vote for the branch, which determines the number of votes (stakes) for different branches in the next voting, i.e. the amount of block generation difficulty parameter Y is determined. Thus, the more votes one branch can get, the faster it will be to generate the next block, and the faster broadcast is helpful to gain more votes in the next round. This process will increase the gap between these two branches and determine the main chain in short time.

The reason why separating the voting for mainchain and transaction record is because it could make stakes decide the branch individually. Because miners will only compete for votes of generating blocks, but votes for mainchain has none business with their income, which makes it not necessary to influence objective of the voting. In addition, it is difficult to control the users because they only need to vote their desired mainchain according to the blockchain data. So even if the users are quite centralized, it also can protect attack to branches effectively. The consensus protocol includes kind of incentive mechanism to the wallet program (refer to “Wallet application” section for more details), which encourages users to select free competition and further ensure the stability of the mainchain.

According to the consensus process above, if the miners would like to get more advantages in the competition, they have to earn more distributed workers to react to random signals from the Voters in the network. Thus, the competition mechanism is impossible to be simulated in single computer, which avoids the competition by only infinitely increasing the performance of mining machines and ensure the mining fairness. benifits<-

Like PoS protocol, we don’t need any external resources. The difference is that when we count the votes, they are already sealed in the past blocks. That makes the “nothing at stake” and “stake grinding” problem resolved. benifits<-

Another difference from PoS is: Because of the existence of the miners, stakeholders could hand over their job to them by the voting process. Miners can help to complete the competition even though the users with low stake are not so positive. Users only need to vote once within a voting cycle to make sure that the holding stakes would not miss the voting activities, which can basically avoid the unequal wealth distribution issue as discussed earlier. benifits<-

Meanwhile, the structure of Miner and Worker can be embedded in App and website development, and I hope it could help some developers to get out of the dilemma in which they only can get revenue with advertisement, or make some contribution to generate better users experience while using the App or website. And there are a big number of developers could be the potential miners. benifits<-

FAQs->

Compete for the Voters
To achieve the ability with faster block generation, the Miners will try the best to compete for as many as possible Voters. My initial design idea is that everybody can use more workers to get votes, like embedding Worker program to the app, if the developers would like to gain more users, they have to upgrade the apps for better user experience progressively. But it is inevitable that some users may reach agreements with each other to cooperate mining as team (e.g. wallet or mining pool). In this case, we would like to provide some alternatives in the transaction structure, which means you can either choose type A – accept vote canvassing from workers with the form of broadcast; or type B- designate one miner and make him win the voting; or even type C – designate yourself and work on mining individually. It looks like that there is some influence on the fairness, but in fact, if we consider from the point of miners, it is beneficial for the healthy of the network if they can compete with each other to attract users via adjusting the distribution plans or other conditions, because eventually there will be diversified unions with different sizes, which compete with regular Miners and Workers for the rights to generate blocks. All these unions or individuals participate in the competition with the mode similar to POS, which could effectively reduce the risk that Miner with tremendous workers would control the voting process.


Wallet application
Here, we would like to discuss sort of special app – wallet application because they can not only guide but also control their users to choose the transaction type and voting targets. However, considering their own benefits, majority of wallet applications will choose type B to mine for themselves, or give higher priority to their own Miners, which voids the chances for other ordinary Miners to participate in the competition. Therefore, there needs a mechanism to encourage wallet applications to select as many type A as possible and to provides wallet applications with a legitimate way to earn income. E.g.: add the field of “wallet income” in type A transactions, and distribute some of the revenues to the operators of wallet applications, but type B and C don’t have this kind of rewards. One extra benefit if we do like this, it can encourage the developers to make better wallet apps or sidechains to provide incentives for open platform. (refer to "Multi chain" section)


PoS variant
We can also derivate a variant for this consensus, i.e. there is only type C. In this case, everybody mines with their own stakes and it becomes pure PoS consensus. This variant system also can ensure the fairness and solves several issues under traditional PoS mode (refer to the section of “Design Background”). Nevertheless, due to the wealth distribution issue which PoS mode can never escape, I personally prefer the original mode, because in term of the stake distribution, miners in type A and B also play important roles (refer to ->).


Reward distribution
If the distribution strategy of mining rewards is different, users may make different behaviors and that will effect the network structure. The distribution we are talking about is only for A type mining, because mining rewards distribution for type B is completed by miners, and there is no distribution requirement for type C mining. And we need to consider some questions as following:

To avoid that the Miner will divide itself to get more benefits, Miner should achieve rewards according to the fixed proportion of total amount.
The stake distribution weight will influence user's choice, and absolute fair distribution mechanism may not be enough to attract more ordinary users if we follow exactly the stake distribution. But if the stake weight is too low, we would lose too many high-stake users.
After decreasing the stake weight, it is necessary to make transaction fee as reference to avoid that users will divide their account infinitely to get more revenues, and it also can provide customers who have different requirements with more strategies to choose.
Therefore, the best solution for distribution plan of type A mining is to be affected by both parameters of stake and transaction fee, e.g. miners share 20% of total rewards, and the rest will be divided into two parts, 1/3 of which is distributed according to transaction fee, and the other 2/3 is based on stakes. (the “stakes” here means accumulated stakes of X parameter, and each Voter could be counted multi times). 25% of rewards from Voters (i.e. 20% of total amount) will be distributed to wallet account.

However, as for distribution plan of type B mining, it is determined by the miner according to the agreement between the Miner and the Voters. It is relatively more free, but there are also some restrictions: to ensure the benefit of wallet applications ( refer to section of “Wallet application”)with type A is always higher than that with type B, B type miner rewards percentage must be restricted under wallet rewards out of type A mining, i.e. under 20%.

The default transaction fee of different voting plans could be adjusted according to the average number of Voters who participated in type A or type B. If this number N is lower, it means there are more miners joined, so we can reduce transaction fee to attract more Voters to match the number of the Miners. Actually, even if there is no adjustment above, the economic principle would also help people to make such balance, just a little slower. In addition, it is also possible to regulate the distribution proportion of the Miners according to the number N: decrease the proportion if there is less people, not only to attract more Voters to select type A , but also to reduce the number of Miners to accelerate the network adjustment.

Frauds and attacks
Filtered transaction, miners only pack the transactions which are beneficial to them
Because the transactions included in each block could influence the competition environment, miners may want to gain more advantages through choosing some of the transactions.
First of all, transactions in each block only take up small percentage of total amount, so there is little influence on the statistic results if some changes are made in one block. To solve this kind of problem better, we need to count the sum of stakes of all the Voters in this transaction, which will be used for the next block generation interval. The higher stake it has, the shorter calculation period it will have, otherwise it is longer. E.g. if we make the calculation period between 0.9-1.1 secs, it would directly affect next block generation. In this case, it would be better choice for packing as many transactions as possible, which could disincentive the miner frauds. This parameter should be adjusted per the average value every once in a while.
Maybe you have one doubt: why we need also refer to y when accumulating the stakes, not directly take x as reference? It is meaningless by logic if we take account into the results of branches when counting the votes. However, there is one unique advantage to do like this. Nobody will get any revenue if they select wrong branch, which facilitate users to be more careful in the branch selection, not just make a choice anyway, or even give up. Although there may have some inaccurate statistics in this case, it could be neglected if compared with the benefits we discussed and there is also very low chance to get orphan block in practice.

It’s also filtered transaction, but the purpose is to influence the branch growth speed
Because the transactions included in each block will also decide the speed of next block generation in the same chain ( influence parameter Y), and attackers could decrease the transactions deliberately in certain branch to affect determination of the mainchain.
Solution: In step 4 of the consensus process, when checking the transaction address b, other than marking the transaction at end of the current chain as y, we also need to mark the transaction that points to the current chain's countdown second, third (quantity adjustable) block, which is y1. Both y and y1 shall be counted when calculating the value of y. In this case, even though the attacker reduces the speed of certain block generation, it will compensate the deliberate missing block if the latter miners are honest. All the transactions will point to the countdown second block and backwards.
Another doubt may come out: is it possible to count transactions marked with xy1 when calculating X parameter? In this case, even though somebody makes fault during packing the transactions, there will be no effect on the results because the latter miners can make compensation for that. The answer is no because if we do like this, the nodes will evade the crucial point when there are forks and vote for the blocks before that to make sure they are right. This can interfere the competition between branches, which makes it hard to determine the main chain promptly.

51% attack
If anyone gains over 50% resources, the system will be totally under his control, which is the same with the other protocols. Here we would like to analyze which level of attack users can launch if under 50% resources.
Let’s remain the same settings as before, the block generation period is 60s, the voting cycle is 6000 blocks interval, then averagely each block has 1/6000 stake of total blocks. In this case, we choose 100 blocks as statistic interval, so averagely in each block generation, there will be (1/6000)*100=1/60 of total stakes involved in the competition. Therefore, if you want to control the block generation, it only requires to put 1/120 stake in each block. For example, suppose one user has 50% stake and hold all of them longer than 100 hours, then he can make sure “double spend” attack within (50%)/(1/120)=60 blocks. In conclusion, if we have a significantly important transaction, wait one hour before we make the decision to ensure absolute safety.
The statistic interval of 100 blocks is a very conservative design because I haven’t started deep research and experiments on the data-access algorithm. To prevent bottlenecks in verifying blocks and transactions, it is temporarily set as 100 blocks. In fact, according to existing database’s search rate, we estimate there is a lot of room for improvement with appropriate design. Suppose the statistic interval could be raised to 1000 blocks, the security of the system will increase by 10 times, and so on.

Simulate Workers, i.e. try to create large number of Worker nodes by simulation and add those virtual nodes to the p2p network to increase the probability of successful canvass. To deal with this situation, we can make some control when creating the p2p network, e.g. every client only needs to build connection with the first M nodes which has the fastest response speed.

Block rate
The same as Bitcoin, we are fully decentralized system, so there will be some restrictions on the block generation speed. However, under this consensus mode, the capability of block generation is actually closely related with network respond time. The Miners with higher chance to generate blocks will have shorter time to receive broadcast of new blocks, while it doesn’t matter much if the Miners which have lower ability of block generation receive the broadcast with some delay. In addition, I switch the main chain selection protocol from POW to GHOST, which can be more effective against attack after producing orphan blocks. Thus, the consensus period in our system may be a little shorter than Bitcoin. (we still lack experimental data for the appropriate block rate, maybe one minute)

If we want to achieve kind of blockchain with high circulation rate in which the blocks are generated in turns among a few nodes, like DPOS, it only needs some modifications in another variant of this consensus. When there are only type B miners in the system, our consensus itself is one kind of voting mechanism and we can modify based on that, e.g. simplify validation process, cancel competition mechanism and reward distribution, cancel the voting of main chain and so on.

General plan of the project
Multi chain
Because of very low cost for merged mining with this consensus protocol, it only needs one more thread to verify one more chain. Also, the computing power attack will not occur since there is no parasitism. Thus, we have more advantages over POW to run side chains.

The degree of decentralization in blockchain determines how robust it is, but it also restricts the speed of network synchronization, which limits the block generation speed as well. Robust and transaction volume become two contradictory features in the blockchain development. On the other hand, the more complex design blockchain has, the more function it can implement, but at the same time, it will bring more insecurity factors due to complicated codes. It is quite difficult to build a perfect blockchain, so it could be a good idea to process data with different requirements by putting them into different chains. Users could store most of their funds in the main chain with full decentralization, more security but lower speed. Then part of their fund will be transferred to certain side chain for micro transactions or other business. Side chain could be designed as high-speed chain which scarifies some security but improves the performance (refer to section of “Block rate”), or more complicated business chain with smart contract, and so on. It could also be a good choice if pegged into other blockchains.

Meanwhile, because it distributes some rewards to developers as incentive ( refer to section of “wallet application”) on the protocol level and hopes this could encourage more developer teams join the development of side chain to bring fresh blood to the healthy growth of the whole system.

Upfront plan
So far, our plan is to get two chains, one of them is used to provide the most secured cryptocurrency protection and trading function, and the other business chain would be implemented as decentralized exchanges (refer to “DEX” for more details). We may give priority to the development of high-throughput micro transaction chain and the business chain that can run smart contracts depending on the team situation and technical details. And optimize the interface standard to make it more convenient to interact with other projects, as mentioned earlier, the target of this project is to construct a scalable multi-functional blockchain and it is certainly essential to realize value transfer between different developers and projects.

FAQs
Q: The system involves in stakes, is it a variant of PoS?
A: no, even though we use stake system, the key philosophy of PonD is to take advantage of network distribution to acquire user's voting, so the competition is through the capactities to achieve votes, but not the stakes. (other differences->) Nevertheless, the PonD consensus can have the same effects with PoS after being simplified. Refer to the section of "PoS variant" for more details.
Q: If Miners collaborate with Voters, they can achieve votes without using network latency. Does it break the fairness of the competition easily?
A: The cooperation can indeed escape the competition of network latency. However, it doesn’t break the fairness because from the perspective of the ability to achieve votes, it is the same result with either network distribution or collaboration. Refer to the section of "Compete for the Voters".
Q: Generally speaking, people may make frauds in the online voting with simulation. How can you prevent that?
A：It makes simulation meaningless when we involve stakes in because nobody can simulate the stakes otherwise the cryptocurrency will not be existed. Also, users are not allowed to gain more voting opportunities by transferring their funds, which has been accomplished in PoS protocol, so we will also follow that. In addition, we have some prevention on the node simulation, please refer to "Frauds and attacks" for more details.
Q: If somebody gathered a large number of users, will that affect the network security?
A: Let's answer the question from two aspects. 1. We will try the best to ensure fair competition among the miners to avoid this centralized situation. See the section of "Compete for the Voters" and "Wallet application";2. We separate the voting of block generation and main china to make sure even if users are very centralized, it can also maintain the network security,refer to ->.
Q：Since the system uses voting mechanism, what is the difference between other voting protocol, like BFT?
A: First of all, the voting logical of BFT is to reach the consensus of "Majority wins", while the voting logical of PonD follows Satoshi Nakamoto's consensus logical which is "able people should get more pay". This is the most essential difference; Secondly, the BFT voting system is among few validation nodes, which is not full distribution system, but voting of PonD can be conducted in any of the nodes, it can be seen as completed distribution system like PoW.
