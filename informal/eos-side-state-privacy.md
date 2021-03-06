# Side state on an EOSIO blockchain for Privacy

November 12, 2018

**Introduction:** This document looks to introduce a concept known as a "side state" to an EOSIO blockchain which could be used to implement features such as SNARKs/STARKs, ring signatures, confidential transactions, etc. which enhance privacy of token transfers within the side state for asset transfers.

**Terminology:**

- `TOK` - A token used to secure the state of the side-chain
- `EOS` - The primary token used on the EOSIO blockchain

## Setup

### Validators / Staking

Staked `TOK` can either be delegated to oneself, or a 3rd party. To be eligible as a validator you must have a stake weight of 1% of `TOK`. 

Validators receive a fee (0.5%/1%) of tokens moving in/out of the side chain as payment for their services. 

If a validator fails to participate, their stake weight is slashed by 10% of its current value. After an 60 minutes, a validator can reclaim the slashed stake. Should a validator's stake weight fall under the 1% threshold, they will temporarily be unable to participate in validation.

### System Contract

A system contract exists such that anyone can deposit funds into the contract, however withdrawing funds from the contract require the 50%+ of the total stake weight approval.

By default, two tokens will be considered approved on the side state: `EOS` and `TOK`.

Additional tokens can be approved if they are `eosio.token` compliant by making a request to the system contract to enable it. After this request has been made, staked `TOK` can vote in favor of the additional token. Once 10% of `TOK` has approved of the additional token it is approved indefinitely and votes can be cleaned from RAM.

If unapproved, the requestor can retract their request clearing it from RAM as well as any votes in favor of the additional token.

## Side State

### Entry

Tokens (`T`) are transferred to the system contract with a side state address as the memo of the transfer action. Once this transfer becomes irreversible on the EOSIO blockchain, tokens (`TD`) are then minted to the side state address at the rate `1 TD = 0.995 TD` via derived consensus. The remaining 0.5% is paid to all current validators.

### Transfer (within side state)

A call is made to the system contract with the transaction data as a parameter. The method used for this on the EOSIO blockchain is a dummy function and "does nothing" as far as the EOSIO blockchain sees.

For example:

```
void transact(account_name account, std::string data) {
	// do nothing
}
```

Pools can be set up such that instead of a user directly calling this function, which would compromise privacy, the pool can make the call on behalf of a user. The pool can request some fee be included as one of the outputs of the transaction as they are using their own CPU/NET resources on the EOSIO blockchain. The rate at which this fee is charged and how it is calculated is up to pool. It is not possible to censor transactions, as a fall back the user can always broadcast the transaction themselves.

Validators using the EOSIO blockchain as a means of transaction sequencing and a known set of rules are then able to derive the current side state unknown to the EOSIO blockchain itself.

### Exit

Tokens (`TD`) are sent to an established side state burn address with the transaction id `TXID` and additional metadata containing an EOSIO blockchain account and memo.

A call is then made to the system contract to request exiting, with the amount of `TD` burned, `TXID` as proof that they have been destroyed within the side state and the memo specified in the metadata. Validators then have a 5 minute period to participate in determining whether this request is valid or not. Validity is determined on the criteria that the amount `TD` was indeed burned by the transaction `TXID`. Validators should refrain from voting until the transaction `TXID` is irreversible.

The vote which triggers approval to go above the 50% threshold will initiate a transfer from the system contract with the account and memo specified in the meta data.

After the 5 minute period has elapsed, assuming there has been above 50% participation, the request can now be "cleaned" by any validator. Cleaning a request releases RAM back to all validators who participated, as well as paying them the 1% exit fee if approved. If the request was approved, it is cleared from RAM and RAM is returned to the requestor.

If a request has failed, after `6*i` hours the requestor can reappeal the same request, where `i` is the number of attempts of reappealing.


### Sanity Check

To ensure the sanity of the side state, one should ensure that the condition `T>=TD` is always met, where `T` is the amount of tokens held by the system contract, and `TD` is the auditable supply of tokens within the side state. The case `T<TD` should never occur unless validators have approved of an invalid exit **and** the "last resort" mechanism (see "Corrupt Validators") has failed.

If the condition `T<TD` holds true, a merchant **should not** accept `TD`.

### Attacks

#### Blockchain Reorganization ("mini forks")
Before coins are minted to the side state (entry) or released from the system contract (exit) we wait for the referenced transaction to become irreversible, we do not need to consider a possible fork in the blockchain.

#### RAM Exhaustion (Exit Transfer)
The system contract will have no additional RAM, so similar to the ["token proxy" solution](https://github.com/EOSEssentials/EOS-Proxy-Token) RAM cannot be exhausted by making the system contract account pay for RAM when transfering tokens to accounts that have no balance of the token.

#### Requestors Exhausting Validators RAM
When an exit is requested, validators all need to contribute some amount of RAM to participate in the vote. However, in all cases, this RAM is returned back to validators after a short 5 minutes. If a requestor proposes invalid exits, each time they do so they will lose RAM which is a scarce resource. An attackers ability to reappeal is limited by their number of attempts as the time needed wait to reappeal will grow by 6 hours with each attempt.

#### Corrupt Validators
The scenario in which an exit which would imbalance the system contracts (`T<TD`) is allowed by validators.

As a "last resort" mechanism, the system contract's active key(s) have the ability to freeze exits from the side state. While frozen, internal transfers and entries can still occur, but exits cannot. The system's active key(s) can also give/revoke authority to other EOS accounts to freeze the system state. However, exits can only be re-enabled by the system contract's active key(s). 

While frozen, an appeal process can be started with authoratative entities such as block producers / ECAF to revoke `TOK` from corrupt validators. As `TOK` is staked, corrupt validators are not able to immediately withdraw/transfer their `TOK` to an exchange and/or other accounts.
