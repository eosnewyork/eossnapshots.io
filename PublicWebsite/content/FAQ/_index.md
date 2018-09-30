---
title: "Frequently asked questions"
date: 2018-06-11T09:07:37-04:00
#pre: "<b>3. </b>"
draft: false
weight: 5
---

#### How are these files generated?

1. A sever side plugin runs within nodeos and writes to a log file every time an account is created.    
2. A script runs each night between 00:01 - 01:00 UTC that collects information about each new account.   
2.1 Only accounts created before midnight that day are considered.  
2.2 Total Account Balance == Liquid EOS + EOS Staked against CPU + EOS Staked against Network + Any EOS pending return to the account via refund/unstaking or liquid tokens. 

#### This seems like a lot of work, why can't I just export the voters table?

A common mistake is thinking that this command will return all accounts.
```
cleos get table eosio eosio voters -l 100
```
Users calling this command will observe that there are account entries in the table that have NOT voted.  

The natural assumption is that the table contains all accounts, those that have NOT voted + those that have.  

This is incorrect, as soon as an account has staked EOS they will show in this table. Since all the Genesis users had their EOS tokens staked, they all show up in this voters table. But, any account created after the Genesis launch of the network that had not staked their EOS tokens and voted, will not be listed in this table. 

At the time of writing (July 29, 2018) our tool exports 273,634 accounts. This is 69,488 more records than the current voter table. 

{{% notice warning %}}
If you're using the voter table as the source of data for your airdrop, you will miss ~70,000 accounts
{{% /notice %}}

#### Should I airdrop to ALL the accounts in the provided files?

Please note that these snapshots also contain system accounts. These can be identified by "eosio.*"

{{% notice info %}}
The below accounts are system accounts. Most users of the data will want to exclude eosio.*
{{% /notice %}}

Examples:

<pre>
eosio.bpay,4392.6925
eosio.msig,0.0000
eosio.names,492950.4560
eosio.ram,2762791.0772
eosio.ramfee,1745467.0855
eosio.saving,4841411.6110
eosio.stake,576904290.7435
eosio.token,3694.8658
eosio.unregd,3301220.3643
eosio.vpay,23619.4660
</pre>

#### How has the data been validated?

The EOS New York team wrote the same export in two different programming languages, Python and .NET. Both applications were run against the same nodeos instance and the outputs were confirmed to be identical. 

#### Is the source code available?

Yes, the following projects are available for review:   
1. Server plugin which exports a global list of accounts - https://github.com/deckb/eos/tree/master/plugins/account_snapshot_plugin  
2. Python script with collects the account information - https://github.com/eosnewyork/eosio/tree/master/scripts/snapshots  
3. .NET core application which collects the account information - https://github.com/eosnewyork/EOSAccountInfo

#### Are EOS that are delegated included in the snapshot?

No, EOS that are delegated to other accounts are not included in either account's balance. 
