[size=16pt][u][b]Our CLAM - The CLAMour Specification[/b][/u][/size]


[hr]

CLAMour will allow the stakeholders of CLAM to record their support for changes to the CLAM network consensus rules.

CLAMour gives the community a method of establishing what changes are supported. 
Supported changes can then be implemented (by anybody) with a high chance of being accepted by the majority of the staking weight.

The process is designed to be decentralized and provable.
There is no need for [i]permission[/i] to propose a change to the network.
It is designed such that any user can 'create' a petition and evangelize to fellow CLAM stakeholders for support.
It will aggregate data on what changes are supported by stakeholders over a time window.

CLAMour happens on-chain using the existing CLAMspeech transaction field so no update to the core software is required to participate. 
An optional update will be provided to make the process more user-friendly.

The following rules are a draft proposal.

[hr]
[hr]


[size=14pt][b]CLAMour Petitions[/b][/size]

CLAMour petitions are specially formatted CLAMspeech messages. 
A petition is a block of free-form text describing the change to be made in sufficient detail to unambiguously implement the change. 
Code isn’t required, but specificity is. 
Each petition is identified by a petition ID, which is the first 8 hex digits of the sha256 hash of its text.

[hr]


[size=14pt][b]Registering A CLAMour Petition[/b][/size]

Before a petition can be voted for, it must be registered on-chain. 
This can be done simply by creating any transaction (coinstake or regular) with a CLAMspeech message saying: 
[code]create clamour <sha256>[/code] where [code]<sha256>[/code] is the full 64 hex digit lowercase sha256 hash of the full petition text. 
The first 8 hex digits of this hash should be unique, since they will be used as the petition ID. 
In the unlikely event of a collision, you’ll need to slightly edit the petition text.

The text itself can be stored wherever you like. 
It is the responsibility of the community supporting the petition to disseminate the petition ID. 
It is planned that the reference client will be able to assist with this in the future.

[hr]


[size=14pt][b]Supporting A CLAMour Petition[/b][/size]

Once a petition has been registered on-chain, anyone can vote for it by staking a block with the CLAMspeech field set to: 
[code]clamour <pid>[/code], where <pid> is the 8 hex digit petition ID of the petition being supported. 
It is possible to register support for multiple petitions at once by listing multiple space-separated petition IDs: 
[code]clamour <pid1> <pid2> <pid3> …[/code]. 
Only CLAMspeech messages associated with staked blocks ('coinstake') will be counted.

Petition support will be counted over a 10,000 block rolling window representing approximately one week’s worth of blocks. 
(60 * 24 * 7 = 10,080).

[hr]


[size=14pt][b]Creating a CLAMour Petition[/b][/size]
[size=12pt]Step-by-Step Instructions[/size]

Write the petition.

[i]If you are using version 1.4.19 of the reference client or newer and wish to use its new GUI feature to create your petition:[/i]
Click the [code]CLAMour[/code] tab in the client.
Enter the petition text into the [code]Create Petition[/code] textbox.
Click the [code]Create Identifier[/code] button to create the sha256 hash of your petition.
Optionally, click the [code]Set Vote[/code] button to immediately begin staking with the new petition ID.
Post the petition text/document, with the attached petition ID to a public forum.

[i]If you are using an older version of the client, or don’t wish to use the new GUI feature:[/i]
sha256 hash the petition text/document. [url=http://www.xorbin.com/tools/sha256-hash-calculator]SHA256 Calculator[/url]
The first 8 hex digits of the hash are the petition ID.
Create a transaction with CLAMspeech set to [code]create clamour <sha256>[/code] where [code]<sha256>[/code] is the full 64 hex digit hash.
Post the petition text/document, with the attached petition ID to a public forum.
It is now up to you to rally support for the petition and prove that the network supports the change.

[hr]


[size=14pt][b]Supporting a CLAMour Petition (voting)[/b][/size]
[size=12pt]Step-by-Step Instructions[/size]

Set your stake(coinstake) CLAMspeech to: 
[code]clamour <pid>[/code], replace [code]<pid>[/code] with the (space-separated) 8 hex digit petition ID(s) you intend to support.
Stake as normal. 
The more blocks you stake, the more votes you will cast.

[hr]


[size=14pt][b]Setting the Stake CLAMspeech Message[/b][/size]
[size=12pt]Step-by-Step Instructions[/size]

In the following, <pid> should be replaced by the (space-separated) list of petition IDs you wish to support.

[i]When using version 1.4.18 of the reference client or older[/i]
There is no way of specifying a default CLAMspeech to be used only when staking, so we have to set a CLAMspeech which is used for all transactions. 
Only speech in staking transactions will be counted as 'votes'.
Either, using the console inside clam-qt: 
[code]setspeech “clamour <pid>”[/code]
Or, using the operating system command line: 
[code]clamd setspeech “clamour <pid>”[/code]

[i]When using version 1.4.19 of the reference client or newer.[/i]
To use the User-Interface:
Click the [code]CLAMour[/code] Tab.
Enter the petition ID(s) you wish to support, one per line, into the [code]Petition ID[/code] textbox.
Click the [code]Set Vote[/code] button.
To use the console or command-line:
Either, using the console inside clam-qt: 
[code]setstakespeech “clamour pid”[/code]
Or, using the operating system command line: 
[code]clamd setstakespeech “clamour pid”[/code]

[hr]


[size=14pt][b]Interpreting the Data[/b][/size]

In the event that two petition texts have the same 8 hex digit petition ID, the one with the “create petition <sha256>” registration speech which appears first in the blockchain is the one being supported by votes. 
This prevents an attacker from faking support for an unpopular petition by brute-forcing a partial hash collision.

A 'majority' exists when greater than 50% of staked blocks over the window express support for a petition.

A 'super-majority' exists when greater than 75% of staked blocks over the window express support for a petition.

These definitions serve to inform our discussion about the data. 
Following discussion, decisions will be made. 
The impact, security, and likelihood of success of any change to the network will be considered. 
