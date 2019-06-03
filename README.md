# token-locking-gov
Token locking governance proof of concept on Ethereum.

# Design overview
- Votes are always binary - either for or against a proposal.
- You vote by staking tokens for periods of time: votes are measured in terms of token-time. You set the amount of time yourself. 
	- Staking means locking the tokens from whatever utility they previously had, including voting. 
	- You can increase the amount of token-time staked but never decrease it during a vote. 
	- There will be max limit on staking time per token of something like 100 years.
	- Staked tokens can still be traded but their value will be different - staked tokens have low fungibility. 
- The winning side's tokens are staked for the token-time they have proposed (skin in the game).
- If you lose the vote, your tokens are not staked, you lose nothing - you never need to be worried about losing a vote. This means that there isn't a disincentive for voting for the losing side. 
- You are rewarded for voting based on an increasing dutch auction mechanism. Offers per vote increase over time for batches of token-time. Once enough token-time has been submitted, the offer per amount of token-time is set and the next batch continues. Each side of the vote has independent auctions and rewards. The side with the lowest average reward per token-time wins the vote. 
- For the vote to end, there is a threshhold in terms of average voting rewards is reached between the two sides of the proposal. Whenever the threshold is reached, there is a further voting period to give everyone the opportunity to vote. If the losing side does not catch up, the vote passes. If the losing side does catch up, the voting continues as before. 

Example winning thresholds:
200x at <0.01%
20x at <0.1% 
2x at <1%

- Rewards for the winning side are given as soon as amount of time the tokens were staked for has finished.
- Anyone can make a new proposal at any time. Trivial proposals will simply be quickly rejected or can be filtered out (see below). 
- Expiry on old proposals is simply a number of days. 

# Design in detail
## Token-time staking
Token staking means that those who make decisions must face the consequences of those decisions (positve or negative), while those who didn't make the decision are able to exit. The voting power is only in terms of: token-time = {tokens staked} x {time staked for}. Any utility the token had is removed for the time that the token is staked. At a minimum, this utility would be the ability to vote. 

However, tokens will still be tradeable even if they are staked. This means that staked tokens will have unique values depending on the time they are staked for and the proposal that was voted for. E.g. if I vote for a risky proposal that passes, my staked tokens will trade at a discount compared to unstaked tokens (excluding any token rewards, see below). 

Trading staked tokens also means that users who wish to continue voting but have staked all their tokens can trade them for unstaked ones. 

The key advantage of using staked token-time for voting rather than simply number of coin holdings, is that you are incentivising users to increase the future utility and value of the token rather than the current value. Well known bribery problems should also be solved because token holders would lose more than they gain by voting for a winning proposal that harms the network.

The maximum amount of staking time will be something like 100 years. This means that users with low coin holding can still heavily influence a vote (and profit, see below). 

## Voting rewards
Analytical readers will have so far noticed that because of the token staking, winning a proposal is actually a cost to the voters since they experience a lack of utility and liquidity for the time their tokens are staked. Therefore, it is necessary to reward winning voters. The rewards will come from inflation of the token supply. Since the reward will be market based, this is simply the cost of governance for the protocol so it makes sense for all token holders to pay for this through supply inflation.

The reward for staking is an amount of tokens proportionate to the amount of coin-time staked. For want of a better name, we can call this the 'reward value'. A market price is achieved for the reward value by using a dutch auction for batches of tokens at increasing reward values. This is done *independently* on both sides of vote. E.g. For the 'for' side of the vote, the first batch might be 100 token-day (e.g. 1 token x 100 days). The auction might start off with a reward value of 0.01 tokens per token-day. This reward value would slowly rise until enough voters have offered to stake at that value. So let's say the final reward value is 0.07 tokens/token-day which means the first batch of 100 token-day was auctioned for 7 tokens. If those voters win, their tokens will be staked for the amount of tokens and time they set and they will receive their reward when the staking of their tokens is complete. 

By having independent dutch auctions, voting is incentivised on both sides of the proposal. This means that if voters are temporarily undervaluing a valuable proposal, it will have a higher reward value which voters can profit from. This creates a market where voters are forced to put a value on the potential effects of a particular proposal and can profit from making good decisions that increase the future value of the token. 

One criticism of this might be that, given that voters want a competitive return on their investment, the typical reward value might end up being quite high. This could be true, but the total amount of tokens that needs to be staked doesn't need to be very high to achieve good decision making, therefore the overall cost should be low. In any case, any token holder will be free to take part in governance therefore, there is no unfair reallocation of value and all value remains in the protocol network. 

## But how do votes end?
The winning side of the vote will be determined by how low the average reward value is across all the token-time staked for each side of the proposal. This can be measured as a ratio between the average reward value on each side of proposal i.e. {trailing side's average reward value} / {leading side's average reward value} (>1 by definition). A high reward value ratio suggests strong demand for the leading side of the proposal. This represents how much value (skin in the game) token holders are willing to stake on a particular outcome (including voting rewards) and also represents the best value-for-money option for the protocol.

To end a vote, reward value ratio thresholds will be used to determine how strong a preference is. The smaller the percentage of available token-time staked, the higher the threshold will be. Example thresholds:

200x at <0.01%
20x at <0.1% 
2x at <1%

After the threshold is reached there is a period of further voting to give everyone the opportunity to vote. If the trailing side of the proposal fails to reduce the reward value ratio to below the threshold value, the leading side wins. If the opposite occurs, voting continues until a threshold is reached again. This also stops bad actors from trying to swing the vote in a paticular direction at the last minute.

Since proposals can be made at any time by anyone, it may be necessary to filter out trivial proposals to stop someone from spamming the network with null proposals. This could simply be done by a fee market for making proposals. However, it's worth bearing in mind that even without filtering, the design may still be able to deal with trivial proposals because the reward value would quickly reach the threshold for rejection at the market rate for staking coin-time on the status quo. There would then be a period of further voting but users would not be forced to check these proposals since they would be rejections of proposals.  

Similarly, it would not be possible to make trivial proposals for profit since there is no value added by voting on trivial proposals therefore the return on investment would tend to a low amount. Nevertheless, other methods to filter out trivial proposals include: 

- A minimum total coin-time staked for the proposal to be rejected or approved.
- Preliminary round where all live proposals have 1 day for voting and only the top 10% of those highest reward value ratios are chosen for 1 further day of voting.
- All of the above.

Having said that, many trivial proposals together may provide value to the network so it may be worth having a very soft filtering approach.

Finally, an expiry time may be applied to all proposals so that ignored or risky proposals that cannot find a conensus are not taken forward.
