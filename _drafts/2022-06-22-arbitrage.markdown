---
layout: post
title:  "The Remarkable Effeciency of Liquidty Pools"
date:   2022-03-05 14:01:45 -0700
categories: Programming
---

A couple of weeks ago I decided I was going to purchase a Ferrari 812 Superfast. Seeing the $401,000 MSRP 
I decided I would turn to the cryptocurrency markets to discover some alpha and subsequently place my order. 

Arbitrage seemed like a promising opportunity so I spent the last couple weeks investigating and constructing 
an algorithmic trading solution for P2P exchange arbitrage. Quickly I discovered this strategy was not the 
money-making, passive income generating, no effort solution I was hoping it would be, and below is an explanation 
of why.

# P2P Exchanges
![Figure 1](/images/p2p.drawio.png)

The first important step in my research, was figuring out exactly how P2P exchanges work. Since P2P exchanges 
do not have any centralized authority or governing body, it's up to the users to fund and operate the protocol. 
This is done by depositing two seperate currencies into a liquidy pool at a certain ratio, in this example, `20:1`.
This is what sets the price for the asset. These so called "liquidity providers" get shares of the Liquidity Pool 
(LP) in exchange for their contribution. 

Traders can now interract with the liquidy pool by swapping assets for a small fee, typically between 0.1% and 0.5%.
The important thing to note here is that the price of the underlying assets depends on the ratio between them 
in the liquidity pool.

Since there is no order book or order flow in automated liquidity protocols, pricing of an asset is determined by 
a _constant product formula_, which can be expressed as `x * y = k` where `x` and `y` are the balances of 
currencies A and B, respectively. `K` is a static factor that 


# L2 Rollup 

<br/>
<small>Figure 1 - A Traditional Load Balanced Application</small>
