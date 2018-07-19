<pre>
Title: eos-forum
Author: asphyxia@novusphere.io
Contributors: Novusphere Discord Community
Updated: 7/18/2018
</pre>

# Abstract
This document looks to outline a Reddit style forum system *built* on the EOS blockchain utilizing the ATMOS token.

# Rationale
Reddit and social media are prone to censorship. A forum built using novusphere-db is trustless and cannot be censored. Anyone can reconstruct the forum database locally by analyzing the EOS blockchain and the interface can be ran entirely local through the use of a local database and interface.

# Anonymity

Users can freely post every 3 minutes in the `anon` sub from the `eosforumanon` account. *This account's* (renewable) resources will be paid for by the Novusphere team.

Users can also stake their atmos, and with **[TBD]** atmos staked, they will be able to post in any sub from the `eosforumanon` account. A signed message will automatically be provided to the eos-service anonymity provider for authentication when posting outside of the anon sub.

# Voting Scheme

### Upvoting
Every user can freely upvote a post for 1 point. **[TBD]** ATMOS can be paid to upvote a post for *an additional?* 1 point. [TBD]% of those ATMOS will go to the person being upvoted, and the remainder will be burned. This is done to prevent circular upvoting abuse.

### Downvoting
**[TBD]** ATMOS can be paid to downvote a post for 1 point. The ATMOS paid to downvote a post are burned.

### Score Formula
Posts will be scored using the formula:

`(p)/(T+2)^G`

- `p` is the net points of the post
- `T` is the time since the post has been made in hours
- `G` is the constant 1.8

# Spam Prevention

### EOS's CPU/Bandwidth
The natural resources of EOS constrain how often a user can post.

### Ignore/Blocked List
Create a list of blocked users stored in local storage (client side) and avoid showing posts by these users. Effectively, this is self-moderation.

*Ignore/Block List of individual posts?* - maybe a user isn't worth ignoring all the time?*

### Proxy Moderation
Opt-in to someone else's ignore/blocked list that they have made public. This would allow you to elect moderators you trust, instead of moderators who may be abusing their position of power. A clear example of this is the contentious moderation of the Reddit sub **r/Bitcoin**. Should you stop trusting someone you have elected as moderator, opt-out of their ignore list.

### Voting
An upvote/downvote system allows the community to *self-police the content they see as valuable and should be given priority*.













