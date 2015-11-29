# Our CLAM - The CLAMour Specification


[hr]

CLAMour will allow the stakeholders of CLAM to record their support for changes to the CLAM network consensus rules.

CLAMour gives the community a method of establishing what changes are supported. 
Supported changes can then be implemented (by anybody) with a high chance of being accepted by the majority of the staking weight.

The process is designed to be decentralized and provable.
There is no need for *permission* to propose a change to the network.
It is designed such that any user can 'create' a petition and evangelize to fellow CLAM stakeholders for support.
It will aggregate data on what changes are supported by stakeholders over a time window.

CLAMour happens on-chain using the existing CLAMspeech transaction field so no update to the core software is required to participate. 
An optional update will be provided to make the process more user-friendly.

The following rules are a draft proposal.

---
---

## CLAMour Petitions

CLAMour petitions are specially formatted CLAMspeech messages. 
A petition is a block of free-form text describing the change to be made in sufficient detail to unambiguously implement the change. 
Code isn’t required, but specificity is. 
Each petition is identified by a petition ID, which is the first 8 hex digits of the sha256 hash of its text.

---

## Registering A CLAMour Petition

Before a petition can be voted for, it must be registered on-chain. 

This can be done simply by creating any transaction (coinstake or regular) with a CLAMspeech message saying: 
`create clamour <sha256>`
where: 
`<sha256>`
is the full 64 hex digit lowercase sha256 hash of the full petition text. 

The first 8 hex digits of this hash should be unique, since they will be used as the petition ID. 
In the unlikely event of a collision, you’ll need to slightly edit the petition text.

The text itself can be stored wherever you like. 
It is the responsibility of the community supporting the petition to disseminate the petition ID. 
It is planned that the reference client will be able to assist with this in the future.

---

## Supporting A CLAMour Petition

Once a petition has been registered on-chain, anyone can vote for it by staking a block with the CLAMspeech field set to: 
`clamour <pid>`
where `<pid>` is the 8 hex digit petition ID of the petition being supported. 

It is possible to register support for multiple petitions at once by listing multiple space-separated petition IDs: 
`clamour <pid1> <pid2> <pid3> …`
Only CLAMspeech messages associated with staked blocks ('coinstake') will be counted.

Petition support will be counted over a 10,000 block rolling window representing approximately one week’s worth of blocks. 
(60 * 24 * 7 = 10,080).

---


## Creating a CLAMour Petition

### Step-by-Step Instructions

- Write the petition.

- If you are using version 1.4.19 of the reference client or newer and wish to use its new GUI feature to create your petition:

  1. Click the tab: `CLAMour`

  2. Enter the petition text into the textbox: `Create Petition`

  3. Click the button: `Create Identifier` to create the sha256 hash of your petition.

  4. Optionally, click the button: `Set Vote` to immediately begin staking with the new petition ID.

  5. Post the petition text/document, with the attached petition ID to a public forum.

- If you are using an older version of the client, or don’t wish to use the new GUI feature:

  1. sha256 hash the petition text/document. [SHA256 Calculator](http://www.xorbin.com/tools/sha256-hash-calculator)

   The first 8 hex digits of the hash are the petition ID.

  2. Create a transaction with CLAMspeech set to: `create clamour <sha256>`

   Where: `<sha256>` is the full 64 hex digit hash.

  3. Post the petition text/document, with the attached petition ID to a public forum. It is now up to you to rally support for the petition and prove that the network supports the change.

---


## Supporting a CLAMour Petition (voting)

### Step-by-Step Instructions

1. Set your stake(coinstake) CLAMspeech to: `clamour <pid>`

2. Replace: `<pid>` with the (space-separated) 8 hex digit petition ID(s) you intend to support.

3. Stake as normal. 

The more blocks you stake, the more votes you will cast.

--- 


## Setting the Stake CLAMspeech Message

### Step-by-Step Instructions

In the following, `<pid>` should be replaced by the (space-separated) list of petition IDs you wish to support.


**When using version 1.4.18 of the reference client or older:**

There is no way of specifying a default CLAMspeech to be used only when staking, so we have to set a CLAMspeech which is used for all transactions. 
Only speech in staking transactions will be counted as 'votes'.

Either, using the console inside clam-qt: `setspeech “clamour <pid>”`

Or, using the operating system command line: `clamd setspeech “clamour <pid>”`


**When using version 1.4.19 of the reference client or newer:**

To use the User-Interface:
1. Click the Tab: `CLAMour`

2. Enter the petition ID(s) you wish to support, one per line, into the textbox: `Petition ID`

3. Click the button: `Set Vote`

To use the console or command-line:

Either, using the console inside clam-qt: `setstakespeech “clamour pid”`

Or, using the operating system command line: `clamd setstakespeech “clamour pid”`

---


## Interpreting the Data

In the event that two petition texts have the same 8 hex digit petition ID, the one with the “create clamour <sha256>” registration speech which appears first in the blockchain is the one being supported by votes. 
This prevents an attacker from faking support for an unpopular petition by brute-forcing a partial hash collision.

A 'majority' exists when greater than 50% of staked blocks over the window express support for a petition.

A 'super-majority' exists when greater than 75% of staked blocks over the window express support for a petition.

These definitions serve to inform our discussion about the data. 
Following discussion, decisions will be made. 
The impact, security, and likelihood of success of any change to the network will be considered. 
