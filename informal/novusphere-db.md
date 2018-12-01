# Novusphere: A Permisionless P2P Database

**Abstract:** A purely peer-to-peer database allows records to be stored in its data set with state transformations preserved via a decentralized blockchain ensuring the records are resistant to censorship. As no individual party has the authority to remove a record from the chain's history, it is assumed no party can be held completely liable for that data. By removing the administrative authority to delete any state transitions, any party can derive the end state of the system ensuring it has not been tampered with. This means applications built using Novusphere can only apply a client-side only moderation policy, such as opt-in black lists to police spam. Assuming the underlying blockchain is not compromised, any individual party can derive their own system state and re-host any Novusphere based application locally removing censorship at the client-side level.

**PLEASE NOTE:** The token "ATMOS" referred to in this document refers to a token on the EOS main net blockchain (`aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906`) under the contract account `novusphereio`.

**DISCLAIMER:** This document is for information purposes only. The Novusphere developers do not guarantee the accuracy of or the conclusions reached in this white paper, and this white paper is provided "as is". The Novusphere developers do not make and expressly disclaims all representations and warranties, express, implied, statutory or otherwise, whatsoever, including, but not limited to: (i) warranties of merchantability, fitness for a particular purpose, suitability, usage, title or non-infringement; (ii) that the contents of this white paper are free from error; and (iii) that such contents will not infringe third-party rights. The Novusphere developers and its affiliates shall have no liability for damages of any kind arising out of the use, reference to, or reliance on this white paper or any of the content contained herein, even if advised of the possibility of such damages. In no event will the Novusphere developers or affiliates be liable to any person or entity for any damages, losses, liabilities, costs or expenses of any kind, whether direct or indirect, consequential, compensatory, incidental, actual, exemplary, punitive or special for the use of, reference to, or reliance on this white paper or any of the content contained herein, including, without limitation, any loss of business, revenues, profits, data, use, goodwill or other intangible losses.

# Background

Censorship on the internet has become a problem as large social media cooperations such as Facebook, Twitter, Reddit, Alphabet (Youtube), etc. have the power to influence their respective user bases through selective enforcement of moderation policies. Existing social media platforms and content sharing platforms work well with the exception they suffer from the inherent weakness that the administrators can exploit their position through selective enforcement of rules. For example, previously we have seen selective enforcement of moderation used to push political biases possibly with the intent of affecting the outcome of presidential elections.

One way to rectify this problem is to remove reliance on a single party to hold/host the data set and instead allow anyone to construct their own cryptographically verified copy and self-host the given application. In this document, we propose how to achieve this by using a peer-to-peer blockchain-based network as the primary data source for the data set. We propose the use of an EOSIO based blockchain, due to its economic model, throughput, and flexibility. In the event the underlying blockchain is ever considered compromised, a snapshot of the system state can be taken and migrated to a new underlying blockchain.

Another problem inherent to centralized social platforms is that their economic models are centered around the harvesting of personal information for the purpose of adversiement. By using these services, users are tacitly agreeing to give up their fundamental privacy rights to systems that time and again have been shown to be insecure and unworthy of the trust users have placed in them. Decentralized open platforms that are not subject to central authority but are instead controlled by its users will naturally be more protective of private information and less prone to monetization schemes that go against their own interests. The community decides the scope of personal information they want exposed, and are by default protected from abuse ingherent to the status quo business model. 

# Token (ATMOS)

ATMOS exists solely as a means of gauging community sentiment and participation in the Novusphere ecosystem. There is no guarantee of current, or future value of ATMOS. Applications built using Novusphere may consider implementing supplementary use cases for ATMOS or distributing new tokens to existing ATMOS holders to fit the application's required economic model.

Changes to the system and/or priority of development can be gauged through weighted coin votes via  ATMOS. However, it is up to the Novusphere developers and/or contributors as to whether they adhere to the results of coin votes. As it is up to the community to decide whether to adopt backwards incompatible changes, it can be assumed that if developers do not adhere to the communities desires, they are developing software that will not be used.

# Database State

Novusphere derives the current state of the system by monitoring an underlying blockchain by using a blockchain, or peer-to-peer distributed timestamp server to generate proof of the chronological order of transactions. For example, Bitcoin uses this to prevent double-spending as the timestamps indicate which transaction occurred first, where the state of the system is the current unspent transaction outputs (UTXO).

Once we know the order of operations (transactions), we can derive the system state. For example, consider the following order of operations:

- a=1, b=0
- a=2
- a=3, b=3
- b=4, c=5

The final state of the system can then be derived as `(a=3, b=4, c=5)`.

Rather than storing the entire content of large data on chain, such as BLOB or files, they can be stored on chain as a pointer/reference. For example, rather than storing a file we can store its IPFS hash on the blockchain, which can then be retrieved through the Novusphere state, which can then be downloaded via the IPFS network. An IPFS hash provides a commitment to data integrity as if the content changes, so does the IPFS hash. As well, once the entire file has been downloaded, the client can recompute the IPFS hash to verify integrity assuming the hash algorithm has not been broken. This system allows Novusphere applications to make use of any data storage/access layer such as IPFS, torrents, HTTP/S, etc. For example, you could store a URL as a pointer/reference on chain, but this would not provide any means to verifying data integrity.

For fast synchronization, snapshots of the system state and history may be used instead of downloading the entire history independently. A hash to verify integrity can be written on chain by 3rd party Novusphere operators who agree that this was indeed a valid state. A new operator may then examine the cumulative total of ATMOS held/staked by these 3rd parties as threshold of whether they should trust the snapshot's integrity or use their own appointed trusted operators as a reference.

# Implementation

The most recent implementation of Novusphere can be found below [on GitHub](https://github.com/Novusphere/nsdb).






