---
title: "Frequently asked questions"
date: 2018-06-11T09:07:37-04:00
#pre: "<b>3. </b>"
draft: false
weight: 5
---

#### How are these files generated?

1. A sever side plugin running within nodeos writes to a log file every time an account is created.    
2. A nightly script then runs through that list and collects information about each account.   
2.1 Only accounts created before midnight that day are considered.  
2.2 Account Balance + EOS Staked against CPU + EOS Staked against Network + Any EOS pending return to the account via refund/unstaking. 

#### This seems like a lot of work, can't I just export the voters table?

A common mistake is thinking that a command like this, will return all accounts
```
cleos get table eosio eosio voters -l 100
```
Users calling this command will observe that there are entries in the table, where that account has NOT voted.  

So the natural assuption is that the table contains all accounts, those that have NOT voted +  those that have.  

This is incorrect, as soon as an account has staked EOS, they will show in this table. Since all Genesis users had their EOS staked, they all show up in this voters table .... but any account created after launch of the network that has not staked EOS / voted, will not be listed in this table. 

At the time of writing our tool exports 273,634 accounts. This is 69,488 more records than the voter table. 

{{% notice warning %}}
If you're using the voter table as the source of data for your airdrop, about 70,000 accounts will miss out on your airdrop.
{{% /notice %}}

#### Should I airdrop to ALL the accounts in the provided files?

On main consideration is the fact that these also contain records for system accounts. These can be identified by "eosio.*"

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

Two EOS New York team members wrote the same export in two differnce programming languages. Both applications were run against the same nodeos instance and the outputs were confirmed to be identical. 

#### Is the sourcode available?

Yes, the following projects are available for review:   
1. Server plugin which exports a global list of accounts - coming soon  
2. Python script with collects the account information - https://github.com/eosnewyork/eosio/tree/master/scripts/snapshots  
3. .NET core application which collects the account information - https://github.com/eosnewyork/EOSAccountInfo
