# Bismuth Mining Evolution

The mainnet of the Bismuth cryptocurrency project was launched on May 1st, 2017. The mining algorithm was based on sha224 and is described here: <a href='http://dx.doi.org/10.4173/mic.2017.4.1'>http://dx.doi.org/10.4173/mic.2017.4.1</a>. In the beginning there were only CPU miners, but after less than 6 months the first GPU miners appeared and shortly afterwards the first GPU mining pools. Bismuth was listed on the Cryptopia exchange during October 2017, and that led to a large increase in new accounts on the network. By January 2018 the number of Bismuth accounts had quadrupled compared to before the exchange listing.

Since Bismuth had a relatively simple mining algorithm requiring very little memory on the GPUs, the network was vulnerable to a 51% attack by a large FPGA or ASIC mining operation. The core development team was well aware of this threat, but decided to work on other issues instead, such as general network stability improvements, in addition to new functions and features. The introduction of hypernodes and the side-chain is one example.

During August and September 2018 it became increasingly evident that an FPGA miner had been developed, and that this mining operation was approaching a 51% portion of the overall mining power in the network. The figure below shows the hashrate distribution of the different pools at that time:  
<img src="pools-854660.jpg" alt="Pools before the fork">  

The presence of a large FPGA miner was identified independently by several independent channels: Bismuth network monitoring pages, pools, miners themselves reporting anomalies, regular exchange dumps, internal core team research on FPGA capabilities as well as work with FPGA developers.  

The FPGA operation was alternating between mining on his own account and using Pool 4 in the figure above. It is believed that a very large portion of the hashrate by Pool 4 shown in the chart above was contributed by the FPGA miner operation. After the hypernodes had been successfully launched, the core dev team had to act swiftly, and an evolution of the mining algorithm moved to the top of the priority list, even though this had not been placed on the roadmap which was published a few months earlier. The modified mining algorithm was developed and tested on a private testnet in record time during September 2018. It took less than 3 weeks from the first conceptual ideas until launch of the new mining algorithm. Even with this rapid pace of development, the exchanges and the pools were given 1 week's notice and time to update their nodes.

What made the FPGA mining so efficient was the fact that the legacy Bismuth Mining algo required only processing power, but no memory. After careful research and tests, one of the core devs, EggdraSyl, came up with a slight change to the current mining algo that:
- Is memory hard
- Would block or penalize the specific FPGA miner a lot
- Still is fast to verify on nodes
- Needs only minimal change to current GPU miners, so it can be implemented quickly by pools  

The "Bismuth Heavy 3" mining algorithm was born, and would be used after the fork.  

On October 8, 2018 (block height 854,660) the new and novel Heavy3 mining algorithm was introduced on the Bismuth mainnet. Previously the Bismuth mining algorithm was computationally expensive, but required little memory. In order to make the new mining algorithm more resistant to FPGAs and ASICs, a requirement to hold a 1GB random binary file in memory was introduced, as illustrated with the yellow boxes in the chart below:  

<img src="Bismuth-anneal2.jpg" alt="New Bismuth mining algo">  

A few days after the new mining algorithm was introduced the distribution of the hashrate among the pools was as shown below:  
<img src="pools-863318.jpg" alt="Pools after the fork">  

After the hardfork, the hashrate from the FPGAs disappeared from the network. The three remaining operating pools were all GPU pools only.

The difficulty plot before and after the hardfork is shown in the plot below:   
<img src="diffhist-hf.png" alt="Diff before and after hf">  

Since it was expected that about 50% of the hashrate would disappear after the hardfork, because of the new, memory intensive algorithm, a difficulty drop down to 108.9 was hard-coded into the node. As seen from the plot above, it took about 6 days (9000 blocks) before the difficulty level stabilized at 111.0. This level is more or less the same as before the hardfork. It is believed that the 40-50% hashrate which was previously provided by the FPGAs, was compensated by GPU miners (both new and previous miners which had left) returning to Bismuth. The hardfork had a positive effect on the number of accounts, which can be seen in the plot below:  

<img src="N_accounts.jpg" alt="Number of accounts">  

## Bismuth Heavy3

The idea behind the "Heavy3" algo designed by EggdraSyl is both simple and effective: It requires a read from a random offset in a fixed lookup table, for each tested nonce.

This concept can be applied to any other mining algorithm as an additional layer to protect against a similar attack.
If the matching algorithm uses hashcash or not (bismuth does not) is irrelevant. The final hash state that is tested is a vector of 32 bits words. Since it's a hash result, it can be considered as a random vector, it can contain anything, and you can not reverse the process - this is a hash core property -
The lookup table also contains random data. For each nonce, the extra step is applying a XOR transform to the hash output, given a random vector from the lookup table, with the index begin determined by the hash itself, therefore at a random, non predictable, location.
The result - xor'd hash state - is considered as the input vector to the difficulty matching function.

- This transformation does not affect the probability of finding a good candidate
- it does not change the hashing algorithm itself
- it does not change the difficulty matching algorithm either.
- It requires reading of about 8 words from a random index for every tried nonce
- The miner has to keep a copy of the whole lookup table in memory at all time

This is then a generic tweak that can be applied instantly to any other crypto.

## Mid and Long Term Considerations

The core team is still in favor of FPGAs and - why not - dedicated ASICs hardware for Bismuth.  

- This is the only way a PoW coin can protect its network.  
  Pure GPU coins always are at the mercy of a nicehash or similarly rented hash attack.
- Hash/Watt of FPGAs and ASICs is way better han GPUs, so you have better efficiency and more hash, a more secure network with more resources needed to take over.

This is only true if the mining equipment is largely available. It's not when a single operation has thousands of custom proprietary hardware. 

Next step will be introducing **several mining channels**, so that everyone has a fair chance to mine, reach profitability, and contribute to the network safety (CPUs, GPUs, FPGAs, ASICs). This would also allow for faster algo changes should a similar situation arise again.
