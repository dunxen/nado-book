\newpage
## Taproot activation

By the time you're reading this the Taproot softfork has activated on the bitcoin mainnet. This happened on November 13th, 2021, approximately one year after its finalized code was merged.^[<https://github.com/bitcoin/bitcoin/commit/3caee16946575e71e90ead9ac531f5a3a1259307>] It happened much quicker than SegWit and with far less drama. But it was not an uneventful year.

In this chapter we first discuss how softforks were activated in the past, and what options were reasonably expected to be available for Taproot. We then zoom in on a heated discussion over `LOT=true` vs `LOT=false` and the "compromise" known as Speedy Trial. Don't worry if these terms mean nothing to you at the moment. Finally we talk about what needs to happed after activation, some of which has already been done as you're reading this.

### Activation options

![Ep. 03 {l0pt}](qr/03.png)

break down and explain the different opinions and options available for activating Taproot and potentially future softfork upgrades.

Helpful Links:

Explainer article by Aaron discussing the different upgrade options:
https://bitcoinmagazine.com/articles/bip-8-bip-9-or-modern-soft-fork-activation-how-bitcoin-could-upgrade-next

<!--
At the start of the episode we explain what softforks are in general. It could make sense to move that to a new chapter about softfork activation, right after the SegWit (softfork) chapter.
-->

<!-- Transcript

-->

### BIP 8 and LOT=true vs FALSE

![Ep. 29 {l0pt}](qr/29.png)

<!-- The transcript link is mentioned here so the volunteer effort gets credit. But it can be moved to where it makes sense in the flow. -->

The transcript for the accompanying episode can be found here.^[This transcript was written by Michael Folkson. The site contains many other transcripts from technical Bitcoin podcast, conference talks and even group conversations. Many of them are written by Bryan Bishop a.k.a. Kanzure, quite possibly one of the fastest typists on Earth.  <https://diyhpl.us/wiki/transcripts/bitcoin-magazine/2021-02-26-taproot-activation-lockinontimeout/>]


discuss activation of the Taproot soft fork upgrade, and more specifically, the lock-in on timeout (LOT) parameter. The LOT parameter can be set to either “true” (LOT=true) or “false” (LOT=false).

LOT=false resembles how several previous soft forks were activated. Miners would have one year to coordinate Taproot activation through hash power; if and when a supermajority (probably 90 percent) of miners signal readiness for the upgrade, the soft fork will activate. But if this doesn’t happen within (probably) a year, the upgrade will expire. (After which it could be redeployed.)LOT=true also lets miners activate the soft fork through hash power, but if they fail to do this within that year, nodes will activate the soft fork regardless.Aaron and Sjors discuss the benefits and detriments of each option. This also includes some possible scenarios of what could happen if some users set LOT to true, while other users set LOT to false, and the associated risks. Finally, Aaron and Sjors discuss what they think is most likely going to happen with Taproot activation.

<!--

### Intro

Aaron van Wirdum (AvW): Live from Utrecht, this is the van Wirdum Sjorsnado. Sjors, make the pun.

Sjors Provoost (SP): We have a “lot” to talk about.

AvW: We have a “lot” to discuss. In this episode we are going to discuss the Taproot activation process and the debate surrounding it on the parameter lot, lockinontimeout which can be set to true and false.

SP: Maybe as a reminder to the listener we have talked about Taproot in general multiple times but especially in Episode 2. And we have talked about activating Taproot, activating soft forks in general in Episode 3 so we might skip over a few things.

AvW: In Episode 3 we discussed all sorts of different proposals to activate Taproot but it has been over half a year at least right?

SP: That was on September 25th so about 5 months, yeah.

AvW: It has been a while and by now the discussion has reached its final stage I would say. At this point the discussion is about the lot parameter, true or false. First to recap very briefly Sjors can you explain what are we doing here? What is a soft fork?

### What is a soft fork?

SP: The idea of a soft fork is you make the rules more strict. That means that from the point of view of a node that doesn’t upgrade nothing has changed. They are just seeing transactions that are valid to them from the nodes that do upgrade. Because they have stricter rules they do care about what happens. The nice thing about soft forks is that as a node user you can upgrade whenever you want. If you don’t care about this feature you can upgrade whenever you want.

AvW: A soft fork is a backwards compatible protocol upgrade and the nice thing about it is that if a majority of miners enforce the rules then that automatically means all nodes on the network will follow the same blockchain.

SP: That’s right. The older nodes don’t know about these new rules but they do know that they’ll follow the chain with the most proof of work, as long as it is valid. If most of the miners are following the new rules then most of the proof of work will be following the new rules. And so an old node will by definition follow that.

AvW: The nice thing about soft forks is that if a majority of hashpower enforces the new rules the network will remain in consensus. Therefore the last couple of soft forks were activated through hash power coordination. That means that miners could include a bit in the blocks they mined signaling that they were ready for the upgrade. Once most miners, 95 percent in most cases, indicated that they were ready nodes would recognize this and enforce the upgrade.

SP: That’s right. A node would check, for example every two weeks, how many blocks signaled this thing and if yes, then it says “Ok the soft fork is now active. I am going to assume that the miners will enforce this.”

### The ability for miners to block a soft fork upgrade

AvW: Right. The problem with this upgrade mechanism is that it also means miners can block the upgrade.

SP: Yeah that’s the downside.

AvW: Even if everyone agrees with the upgrade, for example in this case Taproot, it seems to have broad consensus, but despite that broad consensus miners could still block the upgrade, which is what happened with SegWit a couple of years ago.

SP: Back then there was a lot of debate about the block size and lots of hard fork proposals and lots of hurt feelings. Eventually it was very difficult to get SegWit activated because miners were not signaling for it, probably mostly intentionally. Now it could also happen that miners just ignore an update, not because they don’t like it, just because they’re busy.

AvW: Yeah. In the case of SegWit that was in the end resolved through UASF, or at least that was part of it. We are not going to get into that in depth. That basically meant that a group of users said “On this day (some date in the future, it was August 1st 2017) we are going to activate the SegWit rules no matter how much hash power supports it.”

SP: Right, at the same time and perhaps as a consequence as that, a group of miners and other companies agreed that they would start signaling for SegWit. There were a whole bunch of other things going on at the same time. Whatever happened on 1st August, the thing activated, or a little bit earlier I think.

### The lockinontimeout (LOT) parameter

AvW: Now we are four years ahead in time, it is four years later and now the Taproot upgrade is ready to go. What happened a couple of years ago is now spurring new debate on the Taproot upgrade. That brings us to the lockinontimeout (LOT) parameter which is a new parameter. Although it is inspired by things from that SegWit upgrade period.

SP: It is basically a built in UASF option which you can decide to use or not. There is now a formal way in the protocol to do this to activate a soft fork at a cut off date.

AvW: LOT has two options. The first option is false, LOT is false. That means that miners can signal for the upgrade for one year and then in that year if the 90 percent threshold for the upgrade is met it will activate as we just explained. By the way 1 year and 90 percent isn’t set in stone but it is what people seem to settle on. For convenience sake that is what I’m going to use for discussing this. Miners have 1 year to activate the upgrade. If after that year they have not upgraded the Taproot upgrade will expire. It will just not happen, that is LOT is false.

SP: And of course there is always the option then of shipping a new release, trying again. It is not a “no” vote, it is just nothing happens.

AvW: Exactly. Then there is LOT=true which again miners have 1 year to signal support (readiness) for the upgrade. If a 90 percent threshold is met then the upgrade will activate. However the big difference is what happens if miners don’t reach this threshold. If they don’t signal for the upgrade. In that case when the year is almost over nodes that have LOT=true will start to reject all blocks that don’t signal for the upgrade. In other words they will only accept blocks that will signal for the upgrade which means of course that the 90 percent threshold will be met and therefore Taproot, or any other soft fork in this mechanism, will activate.

SP: If enough blocks are produced.

AvW: If enough blocks are produced, yes, that’s true. A little bit of nuance for those who find it interesting, even LOT=true nodes will accept up to 10 percent of blocks that don’t signal. That’s to avoid weird chain split scenarios.

SP: Yeah. If it activates in the normal way only 90 percent has to signal. If you mandate signaling then it would be weird to have a different percentage suddenly.

AvW: They are going to accept the first 10 percent of non-signaling blocks but after that every block that doesn’t signal is going to be rejected. So the 90 percent threshold will definitely be reached. The big reason for LOT=-true, to set it to true, is that this way miners cannot block the upgrade. Even if they try to block the upgrade, once the year is over nodes will still enforce Taproot. So it is guaranteed to happen.

SP: If enough blocks are produced. We can get into some of the risks with this but I think you want to continue explaining a bit.

AvW: The reason some people like LOT=true is because that way miners don’t have a veto. The counterargument there, you already suggested that, is that miners don’t have a veto anyway even if we use LOT=false the upgrade will expire after a year but after that year we can just deploy a new upgrade mechanism and a new signaling period. This time maybe use LOT=true.

SP: Or even while this thing is going on. You could wait half a year with LOT=false and half a year later say “This is taking too long. Let’s take a little more risk and set LOT=true.” Or lower the threshold or some other permutation that slightly increases the risk but also increases the likeliness of activation.

AvW: Yeah, you’re right. But that is actually also one of the arguments against using LOT=false. LOT=true proponents say that as you’ve suggested we can do it after 6 months, but there are another group of users that might say “No. First wait until the year is over and then we will just redeploy again.” Let’s say after 6 months Taproot hasn’t activated. Now all of a sudden you get a new discussion between people that want to start deploying LOT=true clients right away and groups of users that want to wait until the year is over. It is reintroducing the discussion we are having now except by then we only have 6 months to resolve it. It is like a ticking time bomb kind of situation.

SP: Not really to resolve it. If you don’t do anything for 6 months then there is only one option left which is to try again with a new activation bit.

AvW: But then you need to agree on when you are going to do that. Are you going to do that after 6 months or are you going to do that later? People might disagree.

SP: Then you’d be back to where we are now except you’d know a little bit more because now you know that the miners weren’t signaling.

AvW: And you don’t have a lot of time to resolve it because after 6 months might happen. Some group of users might run LOT=true or….

SP: The thing you’re talking about here is the possibility of say anarchy in the sense that there is no consensus about when to activate this thing. One group, I think we discussed this at length in the third episode, just gets really aggressive and says “No we are going to activate this earlier.” Then nobody knows when it is going to happen.

AvW: Let me put it differently. If right now we are saying “If after 6 months miners haven’t activated Taproot then we will just upgrade to LOT=true clients” then LOT=true proponents will say “If that’s the plan anyways let’s just do it now. That’s much easier. Why do we have to do that halfway?” That is the counterargument to the counterargument.

SP: I can see that. But of course there is also the scenario where we never do this, Taproot just doesn’t activate. It depends on what people want. There is something to be said for a status quo bias where you don’t do anything if it is too controversial for whatever reason. There is another side case here that is useful to keep in mind. There might be a very good reason to cancel Taproot. There might be a bug that is revealed after.

AvW: You are getting ahead of me. There are a bunch of arguments in favor of LOT=false. One argument is we’ve already done LOT=false a bunch of times, the previous miner activated soft forks, and most of the times it went fine. There was just this one time with SegWit in the midst of a big war, we don’t have a big war now. There is no reason to change what we’ve been doing successfully until now. That is one argument. The counterargument would for example be “Yeah but if you choose LOT=false now that could draw controversy itself. It could be used to drive a wedge. We are not in a war right now but it could cause a war.”

SP: I don’t see how that argument doesn’t apply to LOT=true. Anything could cause controversy.

AvW: That is probably fair. I tend to agree with that. The other argument for LOT=false is that miners and especially mining pools have already indicated that they are supportive of Taproot, they’ll activate it. It is not necessary to do the LOT=true thing as far as we can tell. The third argument is what you just mentioned. It is possible that someone finds a bug with Taproot, a software bug or some other problem is possible. If you do LOT=false it is fairly easy to just let it expire and users won’t have to upgrade their software again.

SP: The only thing there is that you’d have to recommend that miners do not install that upgrade. It is worth noting, I think we pointed this out in Episode 3, people don’t always review things very early. A lot of people have reviewed Taproot code but others might not bother to review it until the activation code is there because they just wait for the last minute. It is not implausible that someone very smart starts reviewing this very late, perhaps some exchange that is about to deploy it.

AvW: Something like that happened with P2SH, the predecessor to P2SH. OP_EVAL was just about to be deployed and then a pretty horrible bug was found.

SP: We’ve seen it with certain altcoins too, right before deployment people find zero days, either because they were… or just because the code was shipped in a rush and nobody checked it. There is definitely always a risk, whatever soft fork mechanism you use, that a bug is discovered at the last minute. If you are really unlucky then it is too late, it is deployed and you need a hard fork to get rid of it which would be really, really bad.

AvW: I don’t think that’s true. There are other ways to get rid of it.

SP: Depending on what the bug is you may be able to soft fork it out.

AvW: There are ways to fix it even in that case. The other counterargument to that point would be “If we are not sure that it is bug free and we are not sure that this upgrade is correct then it shouldn’t be deployed either way, LOT=true or LOT=false or anything. We need to be sure of that anyway.”

SP: Yeah but like I said some people won’t review something until it is inevitable.

AvW: I am just listing the arguments. Fourth argument against LOT=true is that LOT=true could feed into the perception that Bitcoin and especially Bitcoin Core developers control the protocol, have power of the protocol. They are shipping code and that necessarily becomes the new protocol rules in the case of Taproot.

SP: There could be some future soft fork where really nobody in the community cares about it, just a handful of Bitcoin Core developers do, and they force it onto the community. Then you get a bunch of chaos. The nice thing about having at least the miners signal is that they are part of the community and at least they are ok with it. The problem is it doesn’t reflect what other people in the community think about it. It just reflects what they think about it. There are a bunch of mechanisms. There is discussion on the mailing list, you see if people have problems. Then there is miner signaling which is a nice indication that people are happy. You get to see that, there are as many people consenting as possible. It would be nice if there were other mechanisms of course.

AvW: The other point that Bitcoin Core developers, while they decide which code they include in Bitcoin Core they don’t decide what users actually end up running.

SP: Nobody might download it.

AvW: Exactly. They don’t actually have power over the network. In that sense the argument is bunk but it could still feed into the perception that they do. Even that perception, if you can avoid it maybe that’s better. That is an argument.

SP: And the precedent. What if Bitcoin Core is compromised at some point and ships an update and says “If you don’t stop it then it is going to activate.” Then it is nice if the miners can say “No I don’t think so.”

AvW: Users could say that as well by not downloading it like you said. Now we get to the fifth argument. This is where it gets pretty complex. The fifth argument against LOT=true is that it could cause all sorts of network instability. If it happens that the year is over and there are LOT=true clients on the network it is possible that they would split off from the main chain and there could be re-orgs. People could lose money and miners could mine an invalid block and lose their block reward and all that sort of stuff. The LOT=true proponents argue that that risk is actually best mitigated if people adopt LOT=true.

SP: I’m skeptical, that sounds very circular. Maybe it is useful to explain what these bad scenarios look like? Then others can decide whether a) they think those bad scenarios are worth risking and b) how to make them less likely. Some of that is almost political. You all have these discussions in society, should people have guns or not, what are the incentives, you may never figure that out. But we can talk about some of the mechanics here.

AvW: To be clear, if somehow there would be complete consensus on the network over either LOT=true, all nodes run LOT=true, or all nodes run LOT=false then I think that would be totally fine. Either way.

SP: Yeah. The irony is of course that if there is complete consensus and everybody runs LOT=true then it will never be used. You’re right in theory. I don’t see a scenario where miners would say “We are happy with LOT=true but we are deliberately not going to signal and then signal at the very last moment.”

AvW: You are right but we are digressing. The point is that the really complicated scenarios arise when some parts of the network are running LOT=true, some parts of the network are running LOT=false or some parts of the network are running neither because they haven’t upgraded. Or some combination of these, half of the network has LOT=true, half of the network has neither. That’s where things get very complicated and Sjors, you’ve thought about it, what do you think? Tell me what the risks are.

### The chain split scenario

SP: I thought about this stuff during the SegWit2x debacle as well as the UASF debacle which were similar in a way but also very different because of who was explaining and whether it was a hard fork or a soft fork. Let’s say you are running the LOT=true version of Bitcoin Core. You downloaded it, maybe it was released by Bitcoin Core or maybe you self compiled it, but it says LOT=true. You want Taproot to activate. But the scenario here is that the rest of the world, the miners aren’t really doing this. The day arrives and you see a block, it is not signaling correctly but you want it to signal correctly so you say “This block is now invalid. I am not going to accept this block.” I’m just going to wait until another miner comes with a block that does meet my criteria. Maybe that happens once in every 10 blocks for example. You are seeing new blocks but they are coming in very, very slowly. So somebody sends you a transaction, you want Bitcoin from somebody, they send you a transaction and this transaction has a fee and it is probably going to be wrong. Let’s say you are receiving a transaction from somebody who is running a node with LOT=false. They are on a chain that is going ten times faster than you are, in this intermediate state. Their blocks might be just full, their fees are pretty low, and you are receiving it. But you are on this shorter, slower moving chain so your mempool is really full and your blocks are completely full, so that transaction probably won’t confirm on your side. It is just going to be sitting in the mempool, that is one complexity. That’s actually a relatively good scenario because you don’t accept unconfirmed transactions. You will have a disagreement with your counterparty, you’ll say “It hasn’t confirmed” and they’ll say “It has confirmed”. Then at least you might realize what is going on, you read about the LOT war or whatever. So that’s one scenario. The other scenario is where somehow it does confirm on your side and it also confirms on the other side. That is kind of good because then you are safe either way. If it is confirmed on both sides then whatever happens in a future re-org that transaction is actually on the chain, maybe in a different block. Another scenario could be because there are two chains, one short chain and one long chain, but they are different. If you are receiving coins that are sent from a coinbase transaction on one side or the other then there is no way it can be valid on your side. This can also be a feature, it is called replay protection essentially. You receive a transaction and you don’t even see it in your mempool, you call the other person and say “This doesn’t make any sense.” That’s good. But now suddenly the world changes its mind and says “No we do want Taproot, we do want LOT=true, we are now LOT=true diehards” and all the miners start mining on top of your shorter chain. Your short chain becomes the very long chain. In that case you are pretty happy in most of the scenarios we’ve discussed.

AvW: Sounds good to me.

SP: You had a transaction that was in maybe in your tenth block and on the other side it was in the first block. It is still yours. There were some transactions floating in the mempool for a very long time, they finally confirm. I think you are fairly happy. We were talking about the LOT=true node. As a LOT=true node user, in these scenarios you are happy. Maybe not if you’re paying someone.

AvW: You are starting to make the case for LOT=true Sjors, I know that is not your intention but you are doing a good job at it.

SP: For the full node user who knows what they are doing in general. If you are a full node user and you know what you are doing then I think you are going to be fine in general. This is not too bad. Now let’s say you are a LOT=false user and let’s say you don’t know what you are doing. In the same scenario you are on the longest chain, you are receiving coins from an exchange and you’ve seen these headers out there for this shorter chain. You might have seen them, depends on whether they reach you or not. But it is a shorter chain and it is valid according to you because it is a more strict ruleset. You are fine, this other chain has Taproot and you don’t probably. You are accepting transactions and you are a happy camper but then all of a sudden because the world changes everything disappears from under you. All the transactions you’ve seen confirmed in a block are now back in the mempool and they might have been double spent even.

AvW: Yeah the reason for that is that we are talking about a chain split that has happened. You have a LOT=false node but at any point the LOT=true chain becomes the longest chain then your LOT=false node would still accept that chain. It would still consider it valid. The other way round is not true. But the LOT=false node will always consider the LOT=true chain valid. So in your scenario where you are using Bitcoin on the longest chain, on the LOT=false chain, we’re happy, we received money, we did a hard day’s work and got our paycheck at the end, paid in Bitcoin. We think we’re safe, we got a bunch of confirmations but then all of a sudden the LOT=true chain becomes longer which means your node switches to a LOT=true chain. That money you received on the LOT=false chain which you thought was the Bitcoin chain is just gone. Poof. That is the problem you are talking about.

SP: Exactly.

AvW: I will add to this very briefly. I think this is an even bigger problem for non-upgraded nodes.

SP: I was about to get to that. Now we are talking about the LOT=false people. You could still say “Why did you download the LOT=false version?” Because you didn’t know. Now we are talking about an unupgraded node. For the unupgraded node Taproot does not exist so it has no preference for which of the chains, it will just pick the longest one.

AvW: It is someone in Korea, he doesn’t keep up with discussion.

SP: Let’s not be mean to Korea.

AvW: Pick a country where they don’t speak English.

SP: North Korea.

AvW: Someone doesn’t keep up with their Bitcoin discussion forums, maybe doesn’t read English, doesn’t really care. He just likes this Bitcoin thing, downloaded the software a couple of years ago, put in his hard day’s work, gets paid and the money disappears.

SP: Or his node might be in a nuclear bunker that he put it in 5 years ago under 15 meters of concrete, airgapped, and somehow it can download blocks because it is watching the Blockstream satellite or something but it cannot be upgraded. And he doesn’t know about this upgrade. Which would be odd if you are into nuclear bunkers and full nodes. Anyway somebody is running an out of date node, in Bitcoin we have the policy that you don’t have to upgrade, it is not a mandatory thing. It should be safe or at least relatively safe to run an unupgraded node. You are receiving a salary, the same as the LOT=false person, then suddenly there’s a giant re-org that comes out of nowhere. You have no idea why people bother to re-org because you don’t know about this rule change.

AvW: You don’t see the difference.

SP: And poof your salary just disappeared.

AvW: That’s bad. That’s basically the worst case scenario that no one wants I think.

SP: Yeah. You can translate this also to people who are using hardware wallet software that hasn’t been updated, they are using remote nodes or they are using SPV nodes that don’t check the rules but only check the work. They’ll have similar experiences where suddenly the longest chain changes so your SPV wallet, which we explained in an earlier episode, its history disappears. At least for the lightweight nodes you could do some victim shaming and say “You should be running a full node. If bad things happen you should have run a full node.” But I still don’t think that’s good safety engineering, to tell people “If you don’t use your safety belt in the correct position the car might explode.” But at least for the unupgraded full node that is an explicit case that Bitcoiners want to support. You want to support people not upgrading and not suddenly losing their coins in a situation like this. That is why I’m not a LOT=true person.

### Avoiding a chain split scenario

AvW: That’s what I want to get that. Everyone agrees or at least we both agree and I think most people would agree that this scenario we just painted, that’s horrible, we don’t want that. So the next question is how do you avoid this scenario? That’s also one of the things where LOT=true and LOT=false people differ in their opinions. LOT=false proponents like yourself argue against LOT=true because the chain split was caused by LOT=true and therefore if we don’t want chain splits we don’t want LOT=true and the thing we just described won’t happened. The worst case scenario is that we don’t have Taproot, it will just expire. That’s not as bad as this poor Korean guy losing his honest day’s work.

SP: Exactly and we might have Taproot later.

AvW: The LOT=true proponents will argue Bitcoin is an open network and any peer can run any software they want. For better or worse LOT=true is a thing that exists. If we want to avoid a chain split the best way to avoid that is to make sure that everyone uses LOT=true or at least the majority of miners upgrade in time and LOT=true is the best way to ensure that. Getting critical mass for LOT=true is actually the safer option despite the fact that LOT=true also introduced the risk. If I want to give an analogy it is kind of like the world would be a safer place without nuclear weapons but there are nuclear weapons. It seems like it is safer to actually have one in that case.

SP: I think that analogy breaks down very quickly but yeah I get the idea.

AvW: It is not a perfect analogy I’m sure. The point is LOT=true exists and now we have to deal with that. It might be a better world, a safer place if LOT=true didn’t exist, if UASFs didn’t exist. But it does and now we have to deal with that fact. There is an argument to be made that making sure the soft fork succeeds is actually the best way to go to save that poor Korean guy.

SP: I am always very skeptical of this type of game theory because it sounds rhetorically nice but I’m not sure it is really true. One of the obvious problems is how do you know you’ve reached the entire Bitcoin community. We talked about this hypothetical person in this other country who is not reading Twitter and Reddit, has no idea that this is going on let alone most of the lightweight wallet users. The number of people who use Bitcoin is much, much greater than the number of people who are even remotely interested in these discussions. Also to even explain the risk to those people, even if we could reach them, to explain why they should upgrade, that alone is a rather big challenge. In this episode we roughly try to explain what would go wrong if they don’t upgrade. We can’t just tell them they must upgrade. That violates the idea that you persuade people with arguments and let them decide what they want to do rather than tell them based on authority.

AvW: Keep in mind that in the end all of this is avoided if a majority of hash power upgrades. With LOT=true it actually is any majority would be fine in the end. If miners themselves use LOT=true then they’ll definitely get the longest chain by the end of the year.

SP: The game theory is kind of narrowed to say you want to convince the miners to do this. The problem though is if it fails we just explained the disaster that happens. Then the question is what is that risk? Can you put a percentage on that? Can you somehow simulate the world and find out what happens?

AvW: I am on the fence on this. I see compelling arguments on both sides. I was leaning towards lot=false at first but the more I think about it… The argument is if you include lot=true in Bitcoin Core then that practically guarantees that everything will be fine because the economic majority will almost certainly run it. Exchanges and most users.

SP: I’m not even sure that is true. That assumes that this economic majority is quick to upgrade and not ignoring things.

AvW: At least within a year.

SP: There might be companies that are running 3 or 4 year old nodes because they have 16 different s***coins. Even that, I would not assume that. We know from the network in general that lots of people don’t upgrade nodes and one year is pretty short. You can’t tell from the nodes whether they are the economic majority or not. That might be a few critical players that would do the trick here.

AvW: Yes I can’t be sure. I’m not sure. I am speculating, I am explaining the argument. But the opposite is also true, now that lot=true exists some group of users will almost certainly run it and that introduces maybe greater risks than if it was included in Core. That would increase the chances of success for LOT=true, the majority upgrading.

SP: It really depends on who that group is. Because if that group is just some random people who are not economically important then they are experiencing the problems and nobody else notices anything.

AvW: That is true. If it is a very small group that might be true but the question is how small or how big does that group need to be for this to become a problem. They have an asymmetry, this advantage because their chain can never be re-orged away while the LOT=false chain can be re-orged away.

SP: But their chain may never grow so that’s also a risk. It is not a strict advantage.

AvW: I think it is definitely a strict advantage.

SP: The advantage is you can’t be re-orged. The disadvantage is your chain might never grow. I don’t know which of those two…

AvW: It would probably grow. It depends on how big that group is again. That’s not something we can objectively measure. I guess that’s what it comes down to.

SP: Even retroactively we can’t. We still don’t know what really caused the SegWit activation even four years afterwards. That gives you an idea of how difficult it is to know what these forces really are.

AvW: Yes, that’s where we agree. It is very difficult to know either way. I am on the fence about it.

SP: The safest thing to do is to do nothing.

AvW: Not even that. There might still be a LOT=true minority or maybe even majority that might still happen.

SP: Another interesting thought experiment then is to say “There is always going to be a LOT=true group for any soft fork. What about a soft fork that has no community support? What if an arbitrary group of people decides to run their own soft fork because they want to? Maybe someone wants to shrink the coin supply. Set the coin issuance to zero tomorrow or reduce the block size to 300 kilobytes.” They could say “Because it is a soft fork and because I run a LOT=true node, there could be others who run a LOT=true. Therefore it must activate and everybody should run this node.” That would be absurd. There is a limit to this game theory. You can always think of some soft fork and some small community that will say this and completely fail. You have to estimate how big and how powerful this thing is. I don’t even know what the metric is.

AvW: But also how harmful the upgrade is because I would say that is the answer to your point there. If the upgrade itself is considered valuable then there is very little cost for people to just switch to the other chain, the chain that can’t be re-orged and that has the upgrade that is valuable. That’s a pretty good reason to actually switch. While switching to a chain even if it can’t be re-orged that screws with the coin limit or that kind of stuff, that is a much bigger disincentive and also a disincentive for miners to switch.

SP: Some people might say that a smaller block size is safer.

AvW: They are free to fork off, that is also possible. We didn’t even discuss that but it is possible that the chain split is lasting, that it will forever be a LOT=true minority chain and a LOT=false majority chain. Then we have the Bitcoin, Bitcoin Cash split or something like that. We just have two coins.

SP: With the big scary sword of Damocles hanging above it.

AvW: Then maybe a checkpoint would have to be included in the majority chain which would be very ugly.

SP: You could come up with some sort of incompatible soft fork to prevent a re-org in the future.

AvW: Let’s work towards the end of this episode.

SP: I think we have covered a lot of different arguments and explained that this is pretty complicated.

AvW: What do you think is going to happen? How do you anticipate this playing out?

SP: I started looking a little bit at the nitty gritty, one of the pull requests that Luke Dashjr opened to implement BIP 8 in general, not specifically for Taproot I believe. There’s already complexity with this LOT=true thing engaged because you need to think about how the peer-to-peer network should behave. From a principle of least action, what is the least work for developers to do, setting LOT to false probably results in easier code which will get merged earlier. And even if Luke is like “I will only do this if it is set to true” then somebody else will make a pull request that sets it to false and gets merged earlier. I think from a what happens when lazy people, I mean lazy in the most respectful way, what is the path of least resistance? It is probably a LOT=false, just from an engineering point of view.

AvW: So LOT=false in Bitcoin Core is what you would expect in that case.

SP: Yes. And somebody else would implement LOT=true.

AvW: In some alt client for sure.

SP: Yeah. And that might have no code review.

AvW: It is just a parameter setting right?

SP: No it is more complicated because how it is going to interact with its peers and what it is going to do when there’s a chain split etc.

AvW: What do you think about this scenario that neither is implemented in Bitcoin Core? Do you see that happening?

SP: Neither. LOT=null?

AvW: Just no activation mechanism because there is no consensus for one.

SP: No I think it will be fine. I can’t predict the future but my guess is that a LOT=false won’t be objected to as much as some people might think.

AvW: We’ll see then I guess.

SP: Yeah we’ll see. This might be the dumbest thing I’ve ever said.

-->

![BIP 8 flow. Full specification and image source: <https://github.com/bitcoin/bips/blob/master/bip-0008.mediawiki>](taproot/bip8.svg)

\newpage

### Speedy Trial

---
comment: transcript https://diyhpl.us/wiki/transcripts/bitcoin-magazine/2021-03-12-taproot-activation-speedy-trial/
...

![Ep. 31 {l0pt}](qr/31.png)

discuss Speedy Trial, the proposed Taproot activation mechanism that has been gaining traction in recent weeks.

Aaron and Sjors explain that Speedy Trial would give miners three months to signal support for the Taproot upgrade with their hash power. If a supermajority of miners signal support for the upgrade within these thee months, Taproot will activate a couple of months later: six months since the release of the software client that includes the activation logic. If miners don’t signal support within three months, the upgrade will expire, and a new upgrade path can be considered. (It is as of yet not defined what the potential alternative upgrade path would look like.)

Aaron explains that Speedy Trial was born out of a compromise between developers and users who preferred different upgrade mechanisms for the Taproot soft fork, while Sjors details what some of the more technical implementation considerations of Speedy Trial are, like the benefits of using block heights instead of time stamps, and the extended delay between signaling and enforcement. Finally, Aaron and Sjors discuss some of the downsides and risks of Speedy Trial.

00:00 - 4:20 Intro and brief explanation of taproot activation
4:20 - 10:02 How Bitcoin core is going to upgrade to taproot/speedy trial
10:02 - 12:58 LOT=True Client
12:59 - 15:10 Differences between speedy trial and LOT=True
15:10 - 16:20 The naming of the clients
16:20 - 17:44 Potential incompatability with the two clients
17:44 - 23:01 If miners signal readiness after the speedy trial fails
23:01 - 25:06 More potential risks with a soft fork
25:13 - 27:03 Recap of the clients and previous topics
27:03 - 32:02 Potential incompatability (cont.)
32:05 - 34:35 If the majority of miners don't signal when the 18 months are up for LOT=True and the collective wisdom of the market
34:36 - 38:44 Concerns about development process of LOT=True client
38:48 - 43:23 Different activation methods
43:25 - 47:28 Block height vs. block time
42:28 - 51:03 The time warp attack/consensus amongst developers
51:03 - 51:53 Wrapping up

TODO: refer to figure in text

![speedy trial flow](taproot/speedy_trial.svg)

<!--

### Intro

Aaron van Wirdum (AvW): Live from Utrecht this is the van Wirdum Sjorsnado. Sjors, what is your pun of the week?

Sjors Provoost (SP): I actually asked you for a pun and then you said “Cut, re-edit. We are going to do it again.” I don’t have a pun this week.

AvW: Puns are your thing.

SP: We tried this LOT thing last time.

AvW: Sjors, we are going to talk a lot this week.

SP: We are going to get blocked for this.

AvW: We talked a lot two weeks ago. LOT was the parameter we discussed two weeks ago, LOT=true, LOT=false, about Taproot activation. We are two weeks further in and now it seems like the community is somewhat reaching consensus on an activation solution called “Speedy Trial”. That is what we are going to discuss today.

SP: That’s right.

### Speedy Trial proposal

Speedy Trial proposal: https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-March/018583.html

Proposed timeline: https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-March/018594.html

AvW: Should we begin with Speedy Trial, what is Speedy Trial Sjors?

SP: I think that is a good idea to do. With the proposals that we talked about last time for activating Taproot, basically Bitcoin Core would release some software, maybe in April of something, and then the miners will start signaling using that software in I think, August or something. Then they can signal for a year and at the end of the year the whole thing ends.

AvW: That was LOT=true or LOT=false. The debate was on whether or not it should end with forced signaling or not. That’s the LOT=true, LOT=false thing.

SP: The thing to keep in mind is that the first signaling, it would be a while before that starts happening. Until that time we really don’t know essentially. What Speedy Trial proposes is to say “Rather than discussing whether or not there is going to be signaling and having lots of arguments about it, let’s just try that really quickly.” Instead there would be a release maybe around April, of course there’s nobody in charge of actual timelines. In that case the signaling would start much earlier, I’m not entirely sure when, maybe in May or pretty early. The signaling would only be for 3 months. At the end of 3 months it would give up.

AvW: It would end on LOT=false basically.

SP: Yes. It is the equivalent of LOT=false or just how it used to be with soft forks. It signals but only for a couple of months.

AvW: If it isn’t activated within these months by hash power which is probably going to be 90 percent hash power? It is going to require 90 percent hash power to activate Taproot. If it doesn’t happen then the proposal expires and when it expires we can continue our discussion on how to activate Taproot. Or if it activates then what happens?

SP: The thing is because you still want to give miners enough time to really upgrade their software the actual Taproot rules won’t take effect until September or August.

AvW: Miners and actual Bitcoin users.

SP: Yes. You want to give everybody plenty of time to upgrade. The idea is we would start the signaling very quickly. Also miners can signal without installing the software. Once the signal threshold has been reached then the soft fork is set in stone. It is going to happen, at least if people run the full nodes. Then there is still some time for people to upgrade and for miners to really upgrade and run that new software rather than just signal for it. They could run that software but they might not. That is why is sort of ok to release a bit early.

AvW: They should really be running the software if they are signaling?

SP: No. We can get into that later.

AvW: For now, to recap really briefly, Speedy Trial means release the software fairly fast and quickly after it is released start the signaling period for 3 months, which is relatively short for a signaling period. See if 90 percent of miners agree, if they do Taproot activates 6 months after the initial release of the software. If 90 percent of miners don’t activate within 3 months the proposal expires and we can continue the discussion on how to activate Taproot.

SP: We are then back to where we were a few weeks ago but with more data.

### The evolution of the Speedy Trial proposal

AvW: Exactly. I want to briefly touch on how we got here. We discussed the whole LOT=true and LOT=false thing and there appeared to be a gridlock. Some people definitely didn’t want LOT=true, some people definitely didn’t want LOT=false and then a third proposal entered the mix. It wasn’t brand new but it wasn’t a major part of the discussion, a simple flag day. A simple flag day would have meant that the Bitcoin Core code would have included a date in the future or a block height in the future, at which point the Taproot upgrade would activate regardless of hash power up until that point.

SP: I find this an even worse idea. When there is a lot of debate people start proposing stuff.

AvW: I think the reason that we reached this gridlock situation where people feel very strongly about different ideas has a lot to do what happened during the SegWit upgrade. We discussed this before but people have very different ideas of what actually happened. Some people feel very strongly that users showed their muscles. Users claimed their sovereignty, users claimed back the protocol and they basically forced miners to activate the SegWit upgrade. It was a huge victory for Bitcoin users. Then other people feel very strongly that Bitcoin came near to a complete disaster with a fractured network and people losing money, a big mess. The first group of people really likes doing a UASF again or starting with LOT=false and switching to LOT=true or maybe just starting with LOT=true. The people who think it was a big mess, they prefer to use a flag day this time. Nice and safe in a way, use a flag day, none of this miner signaling, miners can’t be forced to signal and all of that. These different views on what actually happened a couple of years ago now means people can’t really agree on a new activation proposal. After a lot of discussion all factions were sort of willing to settle on Speedy Trial even though no one really likes it for a couple of reasons which we will get into. The UASF people, they are ok with Speedy Trial because it doesn’t get in the way of the UASF. If the Speedy Trial fails they will still do the UASF next year. The flag day people are sort of ok because the 3 months doesn’t allow for a big enough window to do the UASF probably. The UASF people have said that that is too fast and let’s do this Speedy Trial.

SP: There is also still the LOT=false, let’s just do soft forks the way we’ve done them before where they might just expire. A group of people that were quietly continuing to work on the actual code that could do that. Just from mailing lists and Twitter it is hard to gauge what is really going on. This is a very short timescale.

AvW: The LOT=false people, this is basically LOT=false just on a shorter timescale. Everyone is sort of willing to settle on this even though no one really likes it.

SP: From the point of view that I’m seeing, I’m actually looking at the code that is being written, what I have noticed is that once the Speedy Trial came out more people came out of the woodwork and started writing code that could actually get this done. Whereas before it was mostly Luke I think writing that one pull request.

AvW: BIP 8?

SP: Yeah BIP 8. I guess we can get into the technical details, what I am trying to say is one thing that shows that Speedy Trial seems like a good idea is that there are more developers from different angles cooperating on it and getting things done a little bit more quickly. When you have some disagreement then people start procrastinating, not reviewing things or not writing things. That’s a vague indicator that this seems to be ok. People are working on it quickly and it is making progress so that is good.

AvW: Some technical details you want to get into?

### Different approaches of implementing Speedy Trial

Stack Exchange on block height versus mix of block height and MTP: https://bitcoin.stackexchange.com/questions/103854/should-block-height-or-mtp-or-a-mixture-of-both-be-used-in-a-soft-fork-activatio/

PR 21377 implementing mix of block height and MTP: https://github.com/bitcoin/bitcoin/pull/21377

PR 21392 implementing block height: https://github.com/bitcoin/bitcoin/pull/21392

SP: The idea of Speedy Trial can be implemented in two different ways. You can use the existing BIP 9 system that we already have. The argument for that would be that’s far less code because it already works. It is just for 3 months so why not just use the old BIP 9 code?

AvW: BIP 9 used dates in the future?

SP: Yes. You can tell when the signaling could start, when the signaling times out. There are some annoying edge cases where if it ends right around the deadline but then there is a re-org and it ends right before the deadline, people’s money might get lost if they try to get into the first Taproot block. This is difficult to explain to people.

AvW: The thing is the signaling happens per difficulty period of 2016 blocks. At least up until now 95 percent of blocks needed to signal support. But these two block periods, they don’t neatly fit into exact dates or anything. They just happen. While the signaling period does start and end on specific dates, that is why you can get weird edge cases.

SP: Let’s do an example there, it is fun to illustrate. Let’s say the deadline of this soft fork is on September 1st, pick a date, for signaling. On September 1st at midnight UTC. A miner mines block number 2016 or some multiple of 2016, that’s when the voting ends. They mine this block one second before midnight UTC. They signal “Yes.” Everyone who sees that block says “Ok we have 95 percent or whatever it is and right before midnight Taproot is active.” They have this automatic script that says “I am now going to put all my savings in a Taproot address because I want to be in the first block and I am feeling reckless, I love being reckless.” Then there is another miner who miners 2 seconds later because they didn’t see that recent block. There can be stale blocks. Their block arrives one second past midnight. It votes positive too but it is too late and so the soft fork does not activate because the signaling was not done before midnight, the deadline. That is the subtlety you get with BIP 9. Usually it is not a problem but it is difficult to explain these edge cases to people.

AvW: It is a bigger problem with shorter signaling periods as well?

SP: Yes of course. If there is a longer signaling period it is less likely that the signal is going to arrive at the edge of a period.

AvW: The threshold, I thought it was going to be 90 percent this time?

SP: That’s a separate thing. First let’s talk about, regardless of the threshold, these two mechanisms. One is based on time, that’s BIP 9, easy because we already have the code for it, the downside is all these weird things that you need to explain to people. Nowadays soft forks in Bitcoin are so important, maybe CNN wants to write about it, it is nice if you can actually explain it without sounding like a complete nerd. But the alternative is to say “Let’s just use this new BIP 8 that was proposed anyway and uses height.” We ignore all the LOT=true stuff but the height stuff is very useful. Then it is much simpler. As of this block height that’s when the signaling ends. That height is always at the edge of these retargeting periods. That’s just easier to reason about. You are saying “If the signaling is achieved by block 700,321 then it happens, or it doesn’t happen.” If there is a re-org, that could still be a problem by the way, there could be a re-org at the same height. But then the difference would be that it would activate because we just made the precisely 95 percent. Then there is a re-org and that miner votes no and then it doesn’t activate. That is an edge case.

AvW: That is also true with BIP 9. You remove one edge case, you have one edge case less which is better.

SP: Right, with BIP 9 you could have the same scenario, exactly one vote, if it is just at the edge one miner vote. But the much bigger problem with BIP 9 is that if the time on the block is 1 second after midnight or before this matters. Even if they are way over the threshold. They might have 99.999 percent but that last block comes in too late and so the entire period is disqualified. With an election you are looking at all the votes. You are saying “It has got 97 percent support, it is going to happen” and then that last block is just too late and it doesn’t happen. It is difficult to explain but we don’t have this problem with height based activation.

AvW: I guess the biggest disadvantage of using BIP 8 is that it is a bigger change as far as code comes.

SP: Yeah but I’ve looked at that code yesterday and wrote some tests for it. Andrew Chow and Luke Dashjr have already implemented a lot of it. It has already been reviewed by people. It is actually not too bad. It looks like 50 lines of code. However, if there is a bug in it it is really, really bad. Just because it is only a few lines of code, it might be safer to use something that is already out there. But I am not terribly worried about it.

### The hash power threshold

AvW: Then there is the hash power threshold. Is it 90 or 95?

SP: What is being implemented now in Bitcoin Core is the general mechanism. It is saying “For any soft fork that you call Speedy Trial you could for example use 90 percent.” But for Taproot the code for Taproot in Bitcoin Core, it just says “It never activates.” That is the way you indicate that this soft fork is in the code but it is not going to happen yet. These numbers are arbitrary. The code will support 70 percent or 95 percent, as long as it is not some imaginary number or more than 100 percent.

AvW: It is worth pointing out that in the end it is always 51 percent effectively because 51 percent of miners can always decide to orphan non-signaling blocks.

SP: And create a mess. But they could.

AvW: It is something to be aware of that miners can always do that if they choose to.

SP: But the general principle that is being built now is that at least we could do a slightly lower threshold. There might be still some discussion on whether that is safe or not.

AvW: It is not settled yet? 90 or 95 as far as you know?

SP: I don’t think so. You could have some arguments in favor of it but we will get into that with the risk section.

AvW: Or we can mention really briefly is that the benefit of having the higher threshold is a lower risk of orphan blocks after activation. That’s mainly the reason.

SP: But because we are doing a delayed activation, there’s a long time between signaling and activation, whereas normally you signal and immediately, or at least within 2 weeks, it activates. Right now it can take much, much longer. That means miners have a longer time to upgrade. There is a little less risk of orphaning even if you have a lower signaling threshold.

### Delayed activation

AvW: True. I think that was the third point you wanted to get at anyway. The delayed activation.

SP: What happens normally is you tally the votes in the last difficulty period. If it is more than whatever the threshold is then the state of the soft fork goes from STARTED, as in we know about it and we are counting, to LOCKED_IN. The state LOCKED_IN will normally last for 2 weeks or one retargeting period, and then the rules actually take effect. What happens with Speedy Trial, the delayed activation part, is that this LOCKED_IN state will go on for much longer. It might go on for months. It is LOCKED_IN for months and then the rules actually take effect. This change is only two lines of code which is quite nice.

### Downsides and risks for this proposal

AvW: Ok. Shall we get to some of the downsides of this proposal?

SP: Some of the risks. The first one we briefly mentioned. Because this thing is deployed quite quickly and because it is very clear that the activation of the rules is delayed, there is an incentive for miners to just signal rather than actually install the code. Then they could procrastinate on actually installing the software. That is fine unless they procrastinate so long that they forget to actually enforce the rules.

AvW: Which sounds quite bad to me Sjors.

SP: Yeah. That is bad, I agree. It is always possible for miners to just signal and not actually enforce the rules. This risk exists with any soft fork deployment.

AvW: Yes, miners can always just signal, fake signal. That has happened in the past. We have seen fake signaling. It was the BIP 66 soft fork where we learnt later that miners were fake signaling because we saw big re-orgs on the network. That is definitely something we would want to avoid.

SP: I think we briefly explained this earlier but we can explain it again. Bitcoin Core, if you use that to create your blocks as a miner, there are some safety mechanisms in place to make sure that you do not create a block that is invalid. However if another miner creates a block that is invalid you will mine on top of it. Then you have a problem because the full nodes that are enforcing Taproot will reject your block. Presumably most of the ecosystem, if this signaling works, will upgrade. Then you get into this whole very scary situation where you really hope that is true. Not a massive part of the economy is too lazy to upgrade and you get a complete mess.

AvW: Yes, correct.

SP: I think the term we talked about is the idea of a troll. You could have a troll user. Let’s say I’m a mean user and I’m going to create a transaction that looks like a Taproot transaction but is actually invalid according to Taproot rules. The way that works, the mechanism in Bitcoin to do soft forks is you have this version number in your SegWit transaction. You say “This is a SegWit version 1 transaction.” Nodes know that when you see a higher SegWit version that you don’t know about…

AvW: Taproot version?

SP: SegWit version. The current version of SegWit is version 0 because we are nerds. If you see a SegWit version transaction with 1 or higher you assume that anybody can spend that money. That means that if somebody is spending from that address you don’t care. You don’t consider the block invalid as an old node. But a node that is aware of the version will check the rules. What you could do as a troll is create a broken Schnorr signature for example. You take a Schnorr signature and you swap one byte. Then if that is seen by an old node it says “This is SegWit version 1. I don’t know what that is. It is fine. Anybody can spend this so I am not going to check the signature.” But the Taproot nodes will say “Hey, wait a minute. That is an invalid signature, therefore that is an invalid block.” And we have a problem. There is a protection mechanism there that normal miners will not mine SegWit transactions that they don’t know about. They will not mine SegWit version 1 if they are not upgraded.

AvW: Isn’t it also the case that regular nodes will just not forward the transaction to other nodes?

SP: That is correct, that is another safety mechanism.

AvW: There are two safety mechanisms.

SP: They are basically saying “Hey other node, I don’t think you want to just give your money away.” Or alternatively “You are trying to do something super sophisticated that I don’t understand”, something called standardness. If you are doing something that is not standard I am not going to relay it. That is not a consensus rule. That’s important. It means you can compile your node to relay those things and you can compile your miner to mine these things but it is a footgun if you don’t know what you are doing. But it is not against consensus. However, when a transaction is in a block then you are dealing with consensus rules. That again means that old nodes will look at it and say “I don’t care. I’m not going to check the signature because that is a higher version than I know about.” But the nodes that are upgraded will say “Hey, wait a minute. This block contains a transaction that is invalid. This block is invalid.” And so a troll user doesn’t really stand a chance to do much damage.

AvW: Because the transaction won’t make it over the peer-to-peer network and even if it does it would only make it to miners that will still reject it. A troll user probably can’t do much harm.

SP: Our troll example of a user that swaps one byte in a Schnorr signature, he tries to send this transaction, he sends it to a node that is upgraded. That node will say “That’s invalid, go away. I am going to ban you now.” Maybe not ban but definitely gets angry. But if he sends it to a node that is not upgraded, that node will say “ I don’t know about this whole new SegWit version of yours. Go away. Don’t send me this modern stuff. I am really old school. Send me old stuff.” So the transaction doesn’t go anywhere but maybe somehow it does end up with a miner. Then the miner says “ I am not going to mine this thing that I don’t know about. That is dangerous because I might lose all my money.” However you might have a troll miner. That would be very, very expensive trolling but we have billionaires in this ecosystem. If you mine a block that is invalid it is going to cost you a few hundred thousand euros, I think at the current prices, maybe even more.

AvW: Yeah, 300,000 something.

SP: If you have 300,000 euros to burn you could make a block like that and challenge the ecosystem, say “Hey, here’s a block. Let me see if you actually verify it.” Then if that block goes to nodes that are upgraded, those will reject it. If that block goes to nodes that are not upgraded, it is fine, it is accepted. But then if somebody mines on top of it, if that miner has not upgraded they will not check it, they will build on top of it. Eventually the ecosystem will probably reject that entire chain and it becomes a mess. Then you really, really, really want a very large majority of miners to check the blocks, not just mine blindly. In general, there are already problems with miners just blindly mining on top of other miners, even for a few seconds, for economic reasons.

AvW: That was a long tangent on the problems with false signaling. All of this would only happen if miners are false signaling?

SP: To be clear false signaling is not some malicious act, it is just a lazy, convenient thing. You say “Don’t worry, I’ll do my homework. I will send you that memo in time, don’t worry.”

AvW: I haven’t upgraded yet but I will upgrade. That’s the risk of false signaling.

SP: It could be deliberate too but that would have to be a pretty large conspiracy.

AvW: One other concern, one risk that has been mentioned is that using LOT=false in general could help users launch a UASF because they could run a UASF client with LOT=true and incentivize miners to signal, like we just mentioned. That would not only mean they would fork off to their own soft fork themselves but basically activate a soft fork for the entire economy. That’s not a problem in itself but some people consider it a problem if users are incentivized to try a UASF. Do you understand that problem?

SP: If we go for this BIP 8 approach, if we switch to using block height rather than timestamps…

AvW: Or flag day.

SP: The Speedy Trial doesn’t use a flag day.

AvW: I know. I am saying that if you do a flag day you cannot do a UASF that triggers something else.

SP: You could maybe, why not?

AvW: What would you trigger?

SP: There is a flag day out there but you deploy software that requires signaling.

AvW: That is what UASF people would be running.

SP: They can run that anyway. Even if there is a flag day they can decide to run software that requires signaling, even though nobody would signal probably. But they could.

AvW: Absolutely but they cannot “co-opt” to call it that LOT=false nodes if there is only a flag day out there.

SP: That’s true. They would require the signaling but the flag day nodes that are out there would be like “I don’t know why you’re not accepting these blocks. There’s no signal, there’s nothing to activate. There is just my flag day and I am going to wait for my flag day.”

AvW: I don’t want to get into the weeds too much but if there are no LOT=false nodes to “co-opt” then miners could just false signal. The UASF nodes are activating Taproot but the rest of the network still hasn’t got Taproot activated. If the UASF nodes ever send coins to a Taproot address they are going to lose their coins at least on the rest of the network.

SP: And they wouldn’t get this re-org advantage that they think they have. This sounds even more complicated than the stuff we talked about 2 weeks ago.

AvW: Yes, that’s why I mentioned I’m getting a little bit into the weeds now. But do you get the problem?

SP: Is this an argument for or against a flag day?

AvW: It depends on your perspective Sjors.

SP: That of somebody who does not want Bitcoin to implode in a huge fire and would like to see Taproot activated.

AvW: If you don’t like UASFs, if you don’t want people to do UASFs then you might also not want LOT=false nodes out there.

SP: Yeah ok, you’re saying “If you really want to not see UASF exist at all.” I’m not terribly worried about these things existing. What I talked about 2 weeks ago, I am not going to contribute to them probably.

AvW: I just wanted to mention that that is one argument against LOT=false that I’ve seen out there. Not an argument I agree with myself either but I have seen the argument.

SP: Accurately what you are saying is it is an argument for not using signaling but using a flag day.

AvW: Yes. Even Speedy Trial uses signaling. While it is shorter, it might still be long enough to throw a UASF against it for example.

SP: And it is compatible with that. Because it uses signaling it is perfectly compatible with somebody deploying a LOT=true system and making a lot of noise about it. But I guess in this case, even the strongest LOT=true proponents, one of them at least, argued that would be completely reckless to do that.

AvW: There are no UASF proponents out there right now who think this is a good idea. As far as I know at least.

SP: So far there are not. But we talked about, in September I think, this cowboy theory. I am sure there is somebody out there that will try a UASF even on the Speedy Trial.

### Speedy Trial as a template for future soft fork activations?

AvW: You can’t exclude the possibility at least. There is another argument against Speedy Trial, I find this argument quite compelling actually, which is we came out of 2017 with a lot of uncertainty. I just mentioned the uncertainty at the beginning of this episode, some of it at least. Some people thought UASF was a great success, some people thought it was reckless. Both are partly true, there is truth in both. Now we have a soft fork, Taproot, that everyone seems to love, users seem to like it, developers seem to like it, miners seem to like it, everyone likes it. The only thing we need to do is upgrade it. Now might be a very good opportunity to clean up the mess from 2017 in a way. Agree on what soft forks are exactly, what is the best way to deploy a soft fork and then use that. That way it becomes a template that we can use in more contentious times in the future when maybe there is another civil war going or there is more FUD being thrown at Bitcoin. We seem to be in calm waters right now. Maybe this is a really good time to do it right which will help us moving into the future. While Speedy Trial, no one thinks this is actually the right way. It is fine, we need something so let’s do it. It is arguably kicking the can of the really big discussion we need to have down the road.

SP: Yeah, maybe. One scenario I could see is where the Speedy Trial goes through, activates successfully and the Taproot deployment goes through and everything is fine. Then I think that would remove that trauma. The next soft fork would be done in the nice traditional LOT=false BIP 8. We’ll release something and then several months later miners start signaling and it will activate. So maybe it is a way to get over the trauma.

AvW: You think this is a way to get over the post traumatic stress disorder? Let everyone see that miners can actually activate.

SP: It might be good to get rid of that tension because the downside of releasing regular say BIP 8 LOT=false mechanism is that it is going to be 6 months of hoping that miners are going to signal and then hopefully just 2 weeks and it is done. That 6 months where everybody is anticipating it, people are going to go even crazier than they are now perhaps. I guess it is a nice way to say “Let’s get this trauma over with” But I think there are downsides. For one thing, what if in the next 6 months we find a bug in Taproot? We have 6 months to think about something that is already activated.

AvW: We can soft fork it out.

SP: If that is a bug that can be fixed in a soft fork, yes.

AvW: I think any Taproot, you could just burn that type.

SP: I guess you could add a soft fork that says “No version 1 addresses can be mined.”

AvW: Yes exactly. I think that should be possible right?

SP: Yeah. I guess it is possible to nuke Taproot but it is still scary because old nodes will think it is active.

AvW: This is a pretty minor concern for me.

SP: It is and it isn’t. Old nodes, nodes that are released now basically who know about this Speedy Trial, they will think Taproot is active. They might create receive addresses and send coins. But their transactions won’t confirm or they will confirm and then get unconfirmed. They won’t get swept away because the soft fork will say “You cannot spend this money.” It is not anyone-can-spend, it is “You cannot spend this.” It is protected in that sense. I suppose there are soft fork ways out of a mess but that are not as nice as saying “Abort, abort, abort. Don’t signal.” If we use the normal BIP 8 mechanism, until miners start signaling you can just say “Do not signal.”

AvW: Sure. Any final thoughts? What are your expectations? What is going to happen?

SP: I don’t know, I’’m happy to see progress on the code. At least we’ve got actual code and then we’ll decide what to do with it. Thank you for listening to the van Wirdum Sjorsnado.

AvW: There you go.

-->

### LOT=true client

---
comment: transcript https://diyhpl.us/wiki/transcripts/bitcoin-magazine/2021-04-23-taproot-activation-update/
...

![Ep. 36 {l0pt}](qr/36.png)

discussed the final implementation details of Speedy Trial, the Taproot activation mechanism included in Bitcoin Core 0.21.1. Van Wirdum and Provoost also compared Speedy Trial to the alternative BIP 8 LOT=true activation client.

After more than a year of deliberation, the Bitcoin Core project has merged Speedy Trial as the (first) activation mechanism for the Taproot protocol upgrade. Although van Wirdum and Provoost had already covered Taproot, the different possible activation mechanisms and Speedy Trial specifically in previous episodes, in this episode they laid out the final implementation details of Speedy Trial.

00:00 - 4:20 Intro and brief explanation of taproot activation
4:20 - 10:02 How Bitcoin core is going to upgrade to taproot/speedy trial
10:02 - 12:58 LOT=True Client
12:59 - 15:10 Differences between speedy trial and LOT=True
15:10 - 16:20 The naming of the clients
16:20 - 17:44 Potential incompatability with the two clients
17:44 - 23:01 If miners signal readiness after the speedy trial fails
23:01 - 25:06 More potential risks with a soft fork
25:13 - 27:03 Recap of the clients and previous topics
27:03 - 32:02 Potential incompatability (cont.)
32:05 - 34:35 If the majority of miners don't signal when the 18 months are up for LOT=True and the collective wisdom of the market
34:36 - 38:44 Concerns about development process of LOT=True client
38:48 - 43:23 Different activation methods
43:25 - 47:28 Block height vs. block time
42:28 - 51:03 The time warp attack/consensus amongst developers
51:03 - 51:53 Wrapping up

<!--

### Intro

Aaron van Wirdum (AvW): Live from Utrecht this is the van Wirdum Sjorsnado.

Sjors Provoost (SP): Hello

AvW: Sjors, today we have a “lot” more to discuss.

SP: We’ve already made this pun.

AvW: We’ve already made it twice I think. It doesn’t matter. We are going to discuss the final implementation details of Speedy Trial today. We have already covered Speedy Trial in a previous episode. This time we are also going to contrast it to the LOT=true client which is an alternative that has been released by a couple of community members. We are going to discuss how they compare.

SP: That sounds like a good idea. We also talked about Taproot activation options in general in a much earlier episode.

AvW: One of the first ones?

SP: We also talked about this idea of this cowboy mentality where somebody would eventually release a LOT=true client whatever you do.

AvW: That is where we are.

SP: We also predicted correctly that there would be a lot of bikeshedding.

AvW: Yes, this is also something we will get into. First of all as a very brief recap, we are talking about Taproot activation. Taproot is a proposed protocol upgrade for compact and potentially privacy preserving smart contracts on the Bitcoin protocol. Is that a good summary?

SP: Yeah I think so.

AvW: The discussion on how to upgrade has been going on for a while now. The challenge is that on a decentralized open network like Bitcoin without a central dictator to tell everyone what to run when, you are not going to get everyone to upgrade at the same time. But we do want to keep the network in consensus somehow or other.

SP: Yeah. The other thing that can work when it is a distributed system is some sort of conventions, ways that you are used to doing things. But unfortunately the convention we had ran into problems with the SegWit deployment. Then the question is “Should we try something else or was it just a freak accident and we should try the same thing again?”

AvW: I think the last preface before we really start getting into Speedy Trial, I’d like to point out the general idea with a soft fork, a backwards compatible upgrade which Taproot is, is that if a majority of hash power is enforcing the new rules that means that the network will stay in consensus.

SP: Yeah. We can repeat that if you keep making transactions that are pre-Taproot then those transactions are still valid. In that sense, as a user you can ignore soft forks. Unfortunately if there is a problem you cannot ignore that as a user even if your transactions don’t use Taproot.

AvW: I think everyone agrees that it is very nice if a majority of hash power enforces the rules. There are coordination mechanisms to measure how many miners are on board with an upgrade. That is how you can coordinate a fairly safe soft fork. That is something everyone agrees on. Where people start to disagree on what happens if miners don’t actually cooperate with this coordination. We are not going to rehash all of that. There are previous episodes about that. What we are going to explain is that in the end the Bitcoin Core development community settled on a solution called “Speedy Trial.” We already mentioned that as well in a previous episode. Now it is finalized and we are going to explain what the finalized parameters are for this.

SP: There was a slight change.

AvW: Let’s hear it Sjors. What are the finalized parameters for Speedy Trial? How is Bitcoin Core going to upgrade to Taproot?

### Bitcoin Core finalized activation parameters

Bitcoin Core 0.21.1 release notes: https://github.com/bitcoin/bitcoin/blob/0.21/doc/release-notes.md

Speedy Trial activation parameters merged into Core: https://github.com/bitcoin/bitcoin/pull/21686

SP: Starting on I think it is this Sunday (April 25th, midnight) the first time the difficult readjusts, that happens every two weeks, probably one week after Sunday…

AvW: It is Saturday

SP:… the signaling starts. In about two weeks the signaling starts, no earlier than one week from now.

AvW: Just to be clear, that’s the earliest it can start.

SP: The earliest it can start is April 24th but because it only starts at a new difficulty adjustment period, a new retargeting period, it probably won’t start until two weeks from now.

AvW: It will start at the first new difficulty period after April 24th which is estimated I think somewhere early May. May 4th, may the fourth be with you Sjors.

SP: That would be a cool date. That is when the signaling starts and the signaling happens in you could say voting rounds. A voting round is two weeks or one difficulty adjustment period, one retargeting period. If 90 percent of the blocks in that voting period signal on bit number 2, if that happens Taproot is locked in. Locked in means that it is going to happen, imagine the little gif with Ron Paul, “It’s happening.” But the actual Taproot rules won’t take effect immediately, they will take effect at block number 709632.

AvW: Which is estimated to be mined when?

SP: That will be November 12th this year.

AvW: That is going to differ a bit of course based on how fast blocks are going to be mined over the coming months. It is going to be November almost certainly.

SP: Which would be 4 years after the SegWit2x effort imploded.

AvW: Right, nice timing in that sense.

SP: Every date is nice. That’s what the Speedy Trial does. Every two weeks there is a vote, if 90 percent of the vote is reached then that’s the activation date. It doesn’t happen immediately and because it is a “Speedy” Trial it could also fail quickly and that is in August, around August 11th. If the difficulty period after that or before that, I always forget, doesn’t reach the goal, I think it is after…

AvW: The difficulty period must have ended by August 11th right?

SP: When August 11th is passed it could still activate but then the next difficulty period, it cannot. I think the rule is at the end of the difficulty period you start counting and if the result is a failure then if it is after August 11th you give up but if it is not yet August 11th you enter the next round.

AvW: If the first block of a new difficulty period is mined on August 10th will that difficulty period still count?

SP: That’s right. I think that’s one of the subtle changes made to BIP 9 to make BIP 9 easier to reason about. I think it used to be the other way around where you first check the date but if it was past the date you would give up but if it was before the date you would still count. Now I think it is the other way round, it is a bit simpler.

AvW: I see. There is going to be a signaling window of about 3 months.

SP: That’s right.

AvW: If in any difficulty period within that signaling window of 3 months 90 percent of hash power is reached Taproot will activate in November of this year.

SP: Yes

AvW: I think that covers Speedy Trial.

SP: The threshold is 90 percent as we said. Normally with BIP 9 it is 95 percent, it has been lowered to 90.

AvW: What happens if the threshold is not met?

SP: Nothing. Which means anything could still happen. People could deploy new versions of software, try another bit etc

AvW: I just wanted to clarify that.

SP: It does not mean that Taproot is cancelled.

AvW: If the threshold isn’t met then this specific software client will just do nothing but Bitcoin Core developers and the rest of the Bitcoin community can still figure out new ways to activate Taproot.

SP: Exactly. It is a low cost experiment. If it wins we get Taproot. If not then we have some more information as to why we don’t…

AvW: I also want to clarify. We don’t know what it is going to look like yet. That will have to be figured out then. We could start figuring it out now but that hasn’t been decided yet, what the deployment would look like.

### Alternative to Bitcoin Core (Bitcoin Core 0.21.0-based Taproot Client)

Update on Taproot activation releases: https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-April/018790.html

AvW: Another client was also launched. There is a lot of debate on the name. We are going to call it the LOT=true client.

SP: That sounds good to me.

AvW: It derives from this technical and philosophical difference about how soft forks should be activated in the first place. This client uses, you guessed it, LOT=true. LOT=true means that if the signaling window is over, by the end of the signaling window nodes will start to reject any blocks that don’t signal. They only accept blocks that signal. That is the main difference. Let’s get into the specifics of the LOT=true client.

SP: In the beginning it is the same, in principle it is the same. There is a theoretical possibility that it is not if miners do something really crazy. It starts at a certain block height…

AvW: We just mentioned that Bitcoin Core 0.21.1 starts its signaling window on the first difficulty period after April 24th. This LOT=true client will also in practice start its signaling window on the first difficulty period after April 24th except that April 24th isn’t specified specifically. They just picked the specific block height that is expected to be the first one after April 24th.

SP: Exactly, they picked block 681,408.

AvW: It is specified as a block height instead of indirectly through using a date.

SP: But in all likelihood that is going to be the exact same moment. Both Speedy Trial (Core) and the LOT=true client will start the signaling, the voting periods at the same time. The voting periods themselves, they vote on the same bit. They both vote on bit 2. They both have a threshold of 90 percent. Also if the vote is true then it also has a delayed activation. The delayed activation is a block height in both scenarios in both the Speedy Trial (Core) and the LOT=true variant.

AvW: Botha are November 12th, a November activation anyway. If miners signal readiness for Taproot within the Speedy Trial period both of these clients will activate Taproot in November on that exact same date, exact same block.

SP: So in that sense they are identical. But they are also different.

AvW: Let’s get into the first big difference. We already mentioned one difference which is the very subtle difference between starting it at height, the thing we just mentioned. Let’s get into a bigger difference.

SP: There is also a height for a timeout in this LOT=true and that is also a block. But not only is that a block, that could be a slight difference especially when it is a long time period. At the beginning if you use block height or time, date you can guess very accurately but if it is a year ahead then you can’t. But this is almost two years ahead, this block height (762048, approximately November 10th 2022) that they have in there. It goes on much longer.

AvW: Two years from now you mean, well one and a half.

SP: Exactly. In that sense it doesn’t really matter that they are using height because it is such a big difference anyway. But this is important. They will keep signaling much longer than the Speedy Trial. We can get into the implications later but basically they will signal much later.

AvW: Let’s stick to the facts first and the implications later. Speedy Trial (Bitcoin Core), it will last for 3 months. And this one, the LOT=true client will allow signaling for 18 months.

SP: The other big difference is that at the end of that 18 months, where the Speedy Trial will simply give up and continue, the LOT=true will wait for miners who do signal. This could be nobody or could be everybody.

AvW: They will only accept signaling blocks after these 18 months. For those who are aware of the whole block size war, it is a little bit like the BIP 148 client.

SP: It is pretty much the same with slightly better tolerance. The UASF client required every single block to signal whereas this one requires 90 percent to signal. In practice if you are the miners at the last 10 percent of that window you need to pay a bit more attention. Other than that it is the same.

AvW: That is why some people would call this the UASF client. The BIP 148 client was the UASF client for SegWit, this is the UASF client for Taproot. I know that for example Luke Dashjr has been contributing to this client doesn’t like the term UASF in this context because there is 18 months of regular miner signaling.

SP: So did the UASF. It is a bit more patient than the UASF.

AvW: There is a lot of discussion on the name of the client and what people should call it or not call it. In general some people have been calling it the UASF client and this is why.

SP: You could call it the “Slow UASF” or something.

### Implications of having two alternative clients

AvW: I have also seen the name User Enforced Miner Activated Soft Fork (UEMASF). People are coming up with names. The basic facts are clear now I hope. Let’s get into the implications. There are some potential incompatibilities between these two activation clients. Everyone agrees that Taproot is great. Everyone wants Taproot. Everyone agrees that it would be preferable if miners activate it. The only thing where there is some disagreement on is what’s the backup plan. That is where the incompatibilities come in. Do you agree?

SP: I think so.

AvW: What are the incompatibilities? First of all and I already mentioned this, to emphasize this, if Speedy Trial activates Taproot there are no incompatibilities. Both clients are happily using Taproot starting in November. This seems pretty likely because 90 percent of mining pools have already indicated that they support Taproot. Likely there is no big deal here, everything will turn out fine. If Speedy Trial doesn’t succeed in activating Taproot that is where we enter a phase where we are going to start to look at potential incompatibilities.

SP: For sure. Imagine one scenario where Speedy Trial fails. Probably Bitcoin Core people will think about that for a while and think about some other possibilities. For some reason miners get wildly enthusiastic right after Speedy Trial fails and start signaling at 90 percent. As far as Bitcoin Core is concerned Taproot never activated. As far as the UASF or LOT=true client Taproot did just activate.

AvW: Let’s say in month 4, we have 3 months of Speedy Trial and then in month 4 miners suddenly signal readiness for Taproot. Bitcoin Core doesn’t care anymore, Bitcoin Core 0.21.1 isn’t looking at the signaling anymore. But this LOT=true client is. On the LOT=true client Taproot will be activated in November while on this Bitcoin Core client it will not.

SP: Then of course if you are using that LOT=true client and you start immediately using Taproot at that moment because you are very excited, you see all these blocks coming in, you may or may not lose your money. Anybody who is running the regular Bitcoin Core client will accept those thefts from Taproot addresses essentially.

AvW: In this case it matters what miners are doing as well. If miners signal readiness because they are actually ready and they are actually going to enforce Taproot then it is fine. There is no issue because they will enforce the soft fork and even Bitcoin Core 0.21.1 nodes will follow this chain. The LOT=true client will enforce and everybody is happy on the same chain. The only scenario where this is a problem, what you just described, is if miners do signal readiness but they aren’t actually going to enforce the Taproot rules.

SP: The problem is of course in general with soft forks, but especially if everyone is not on exactly the same page about what the rules are, you only know that it is enforced when it is actually enforced. You don’t know if it is going to be enforced in the future. This will create a conundrum for everybody else because then the question is what to do? One thing you could do at that point is say “Obviously Taproot is activated so let’s just release a new version of Bitcoin Core that just says retroactively it activated.” It could just be a BIP 9 soft fork repeating the same bit but slightly later or it could just say “We know it activated. We will just hardcode the flag date.”

AvW: It could just be a second Speedy Trial. Everything would work in that case?

SP: There is a problem with reusing the same bit number within a short period of time. (Note: AJ Towns stated on IRC that this would only be a problem if multiple soft forks were being deployed in parallel.) Because it would be exactly the week after in the scenario we talked about it may not be possible to use the same bit. Then you will have a problem because you can’t actually check that specific bit but there’s no signal on any of the other bits. That would create a bit of a headache. The other solution would be very simple, to say it apparently activated so we will just hardcode the block date and activate it then. The problem is what if between the time that the community decides “Let’s do this” and the moment that software is released and somewhat widely deployed one or more miners say “Actually we are going to start stealing these Taproot coins.” You get a massive clusterf*** in terms of agreeing on the chain. Now miners will not be incentivized to do this because why would you deliberately create complete chaos if you just signaled for a soft fork? But it is a very scary situation and it might make it scary to make the release. If you do make the release but miners start playing these shenanigans what do you do then? Do you accept a huge re-org at some point? Or do you give up and consider it not deployed? But then people lose their money and you’ve released a client that you now have to hard fork from technically. It is not a good scenario.

AvW: It gets complicated in scenarios like this, also with game theory and economics. Even if miners would choose to steal they risk stealing coins on a chain that could be re-orged. They have just mined a chain that could be re-orged if other miners do enforce these Taproot rules. It gets weird, it is a discussion about economic incentives and game theory in that scenario. Personally I think it is pretty unlikely that something like this would happen but it is at least technically possible and it is something to be aware of.

SP: It does make you wonder whether as a miner it is smart to signal immediately after this Speedy Trial. This LOT=true client allows for two years anyway. If the only reason you are signaling is because this client exists then I would strongly suggest not doing it immediately after the Speedy Trial. Maybe wait a little bit until there is some consensus about what to do next.

AvW: One thing you mentioned and I want to quickly address that, this risk always exists for any soft fork. Miners can always false signal, they could have done it with SegWit for example, false signal and then steal coins from SegWit outputs. Old nodes wouldn’t notice the difference. That is always a risk. I think the difference here is that Bitcoin Core 0.21.1 users in this scenario might think they are running a new node, from their perspective they are running an updated node. They are running the same risks as previously only outdated nodes would run.

SP: I would be mostly worried about the potential 0.21.2 users who are installing the successor to Speedy Trial which retroactively activates Taproot perhaps. That group is very uncertain of what the rules are.

AvW: Which group is this?

SP: If the Speedy Trial fails and then it is signaled, there might be a new release and people would install that new release, but then it is not clear whether that new release would be safe or not. That very new release would be the only one that would actually think that Taproot is active, as well as the LOT=true client. But now we don’t know what the miners are running and we don’t know what the exchanges are running because this is very new. This would be done in a period of weeks. Right now we have a 6 month… I guess the activation date would still be November.

AvW: It would still be November so there is still room to prepare in that case.

SP: Ok, then I guess what I said before is nonsense. The easier solution would be to do a flag date where the new release would say “It is going to activate on November 12th or whatever that block height is without any signaling.” Signaling exists but people have different interpretations about it. That could be a way.

### Recap

AvW: I am still completely clear about what we are talking about it here but I am not sure if our listeners are catching up at this point. Shall we recap? If miners activate during the Speedy Trial then everything is fine, everyone is in consensus.

SP: And the new rules take effect in November.

AvW: If miners activate after the Speedy Trial period then there is a possibility that the LOT=true client and the Bitcoin Core 0.21.1 client won’t be in consensus if an invalid Taproot block is ever mined.

SP: They have no forced signaling, you’re right. If an invalid Taproot transaction shows up after November 12th….

AvW:…. and if that is mined and enforced by a majority of miners, a majority of miners must have false signaled, then the two clients can get out of consensus. Technically this is true, I personally think it is fairly unlikely. I am not too concerned about this but it is at least technically true and something people should be aware of.

SP: That scenario could be prevented by saying “If we see this “false” signaling, if we see this massive signaling a week after the Speedy Trial then you could decide to release a flag date client which just says we are going to activate this November 12th because apparently the miners want this. Otherwise we have no idea what to make of this signal.”

AvW: I find it very hard to predict what Bitcoin Core developers in this case are going to decide.

SP: I agree but this is one possibility.

### A likelier way that the two clients could be incompatible?

AvW: That was one way the two clients can become incompatible potentially. There is another way which is maybe more likely or at least it is not as complicated.

SP: The other one is “Let’s imagine that the Speedy Trial fails and the community does not have consensus over how to proceed next.” Bitcoin Core developers can see that, there is ongoing discussion and nobody agrees. Maybe Bitcoin Core developers decide to wait and see.

AvW: Miners aren’t signaling…

SP: Or erratically etc. Miners aren’t signaling. The discussion still goes on. Nothing happens. Then this LOT=true mechanism kicks in…

AvW: After 18 months. We are talking about November 2022, it is a long way off but at some point the LOT=true mechanism will kick in.

SP: Exactly. Those nodes will then, assuming that the miners are still not signaling, they will stop…

AvW: That is if there are literally no LOT=true signaling blocks.

SP: In the other scenario where miners do massively start signaling, now we are back to that previous scenario where suddenly there is a lot of miner signaling on bit 2. Maybe the soft fork is active but now there is no delay. If the signaling happens anywhere after November 12th the LOT=true client will activate Taproot after one adjustment period.

AvW: I am not sure I am following you.

SP: Let’s say in this case in December of this year the miners suddenly start signaling. After the minimum activation height. In December they all start signaling. The Bitcoin Core client will ignore it but the LOT=true client will say “Ok Taproot is active.”

AvW: This is the same scenario we just discussed? There is only a problem if there is a false signaling. Otherwise it is fine.

SP: There is a problem if there is false signaling but it is more complicated to resolve this one because that option of just releasing a new client with a flag day in it that is far enough into the future, that is not longer there. It is potentially active immediately. If you do a release but then suddenly a miner starts not enforcing the rules you get this confusion that we talked about earlier. Then we are able to solve it by just making a flag date. This would be even messier. Maybe it is also even less likely.

AvW: It is pretty similar to the previous scenario but a little more difficult, less obvious how to solve this.

SP: I think it is messier because it is less obvious how you would do a flag day release in Bitcoin Core in that scenario because it immediately activates.

AvW: That is not where I wanted to go with this.

SP: You wanted to go for some scenario where miners wait all the way to the end until they start signaling?

AvW: Yes that is where I wanted to go.

SP: This is where the mandatory signaling kicks in. If there is no mandatory signaling then the LOT=true nodes will stop until somebody mines a block that they’d like to see, a block that signals. If they do see this block that signals we are back in the previous example where suddenly the regular Bitcoin Core nodes see this signaling but they ignore it. Now there is a group of nodes that believe that Taproot is active and there is a group of nodes that don’t. Somebody then has to decide what to do with it.

AvW: You are still talking about false signaling here?

SP: Even if the signaling is genuine you still want there to be a Bitcoin Core release, probably, that actually says “We have Taproot now.” But the question is when do we have Taproot according to that release? What is a safe date to put in there? You could do it retroactively.

AvW: Whenever they want. The point is if miners are actually enforcing the new rules then the chain will stay together. It is up to Bitcoin Core to implement whenever they feel like it.

SP: The problem with this signaling is you don’t know if it is active until somebody decides to try to break the rules.

AvW: My assumption was that there wasn’t false signaling. They’ll just create the longest chain with the valid rules anyway.

SP: The problem with that is that it is unknowable.

AvW: The scenario I really wanted to get to Sjors is the very simple scenario where the majority of miners doesn’t signal when the 18 months are up. In that case they are going to create the longest chain that the Bitcoin Core 0.21.1 nodes are going to follow while the LOT=true nodes are only going to accept blocks that do signal which maybe zero or at least fewer. If it is a majority then there is no split. But if it is not a majority then we have a split.

SP: And that chain would get further and further behind. The incentive to make a release to account for that would be quite small I think. It depends, this is where your game theory comes in. From a safety point of view, if you now make a release that says “By the way we retroactively consider Taproot active” that would cause a giant re-org. If you just activate it that wouldn’t cause a giant re-org. But if you say “By the way we are going to retroactively mandate that signaling that you guys care about” that would cause a massive re-org. This would be unsafe, that would not be something that would be released probably. That is a very messy situation.

AvW: There are messy potential scenarios. I want to emphasize to our dear listeners, none of this is going to happen in the next couple of months.

SP: And hopefully never. We will throw in a few other bad scenarios and then I guess we can go onto some other topics.

AvW: I want to mention real quick is the reason I’m not too concerned about these really bad scenarios playing out is because I think if it seems even slightly likely that there might be a coin split or anything like that there will probably be futures markets. These futures markets will probably make it very clear to everyone the alternative chain that stands a chance, that will inform miners on what to mine and prevent a split that way. I feel pretty confident about the collective wisdom of the market to warn everyone about potential scenarios so it will probably work out fine. That’s my general perception.

SP: The problem with this sort of stuff is if it doesn’t work out fine it is really, really bad. Then we get to say retroactively “I guess it didn’t work out fine.”

### Development process of LOT=true client

AvW: I want to bring something up before you bring up whatever you wanted to bring up. I have seen some concerns by Bitcoin Core developers about the development process of the LOT=true client. I think this gets down to the Gitian building, Gitian signing which we also discussed in another episode.

SP: We talked about the need for software to be open source, to be easy to audit.

AvW: Can you give your view on that in this context?

SP; The change that they made relative to the main Bitcoin Core client is not huge. You can see it on GitHub. In that sense that part of open source is reasonably doable to verify. I think that code has had less review but not zero review.

AvW: Less than Bitcoin Core’s?

SP: Exactly but much more than the UASF, much more than the 2017 UASF.

AvW: More than that?

SP: I would say. The idea has been studied a bit longer. But the second problem is how do you know that what you are downloading isn’t malware. There are two measures there. There is release signatures, the website explains pretty well how to check those. I think they were signed by Luke Dashjr and by the other developer. You can check that.

AvW: Bitcoin Mechanic is the other developer. Actually it is released by Bitcoin Mechanic and Shinobi and Luke Dashjr is the advisor, contributor.

SP: Usually there is a binary file that you download and then there is a file with checksums in it and that file with checksums is also signed by a known person. If you have Luke’s key or whoever, their key and you know them, you can check that at least the binary you downloaded is not coming from a hacked website. The second thing, you just have a binary and you know they signed it but who are they? The second thing is you want to check that this code matches the binary and that is where Gitian building comes in which we talked about in an earlier episode. Basically deterministic builds. It takes the source code and it produces the binary. Multiple people can then sign that indeed according to them this source code produces this binary. The more people that confirm that the more likely it is that they are not colluding. I think there are only two Gitian signatures for this other release.

AvW: So the Bitcoin Core software is being Gitian signed by…

SP: I think 10 or 20 people.

AvW: A lot of the experienced Bitcoin Core developers that have been developing the Bitcoin Core software for a while. Including you? Did you sign it?

SP: The most recent release, yes.

AvW: You are trusting that they are not all colluding and spreading malware. It still comes down to trust in that sense for most people.

SP: If you are really contemplating running this alternative software you really should know what you are doing in terms of all these re-org scenarios. If you already know what you are doing in those terms then just compile the thing from source. Why not? If you are not able to compile things from source you probably shouldn’t be running this. But that is up to you. I am not worried that they are shipping malware but in general it is just a matter of time before somebody says “I have a different version with LOT=happy and please download it here” and it steals all your Bitcoin. It is more the precedent this is setting that I’m worried about than that this thing might actually have malware in it.

AvW: That is fair. Maybe sign it Sjors?

SP: No because I don’t think this is a sane thing to release.

AvW: Fair enough.

SP: That’s just my opinion. Everyone is free to run whatever they want to run.

AvW: Was there anything else you wanted to bring up?

### What would Bitcoin Core release if Speedy Trial failed to activate?

SP: Yeah we talked about true signaling or false signaling on bit 1 but a very real possibility I think if this activation fails and we want to try something else then we probably don’t want to use the same bit if it is before the timeout window. That could create a scenario where you might start saying “Let’s use another bit to do signaling.” Then you could get some confusion where there is a new Bitcoin Core release that activates using bit 3 for example but the LOT=true folks don’t see it because they are looking at bit 1. That may or may not be an actual problem. The other thing is that there could be all sorts of other ways to activate this thing. One could be a flag day. If Bitcoin Core were to release a flag day then there won’t be any signaling. The LOT=true client won’t know that Taproot is active and they will demand signaling at some point even though Taproot is already active.

AvW: Your point being that we don’t know what Bitcoin Core will release after Speedy Trial and what they might release might not necessarily be compatible with the LOT=true client. That works both ways of course.

SP: Sure. I am just reasoning from one point here. I would also say that in the event that Bitcoin Core releases something else that has pretty wide community support, I would imagine the people who are running the BIP 8 clients are not sitting in a cave somewhere. They are probably relatively active users that can decide “I am going to run this Bitcoin Core version again because there is a flag day in it which is earlier than the forced signaling.” I could imagine they would decide to run it or not.

AvW: That also works both ways.

SP: No, not really. I am much more worried about people who are not following this discussion who just default to whatever the newest version of Core is. Or don’t upgrade at all, they are still running say Bitcoin Core v0.15. I am much more worried about that group than about the group that actively takes a stance on this thing. If you actively take a stance by running something else then you know what you are doing. It is up to you to stay up to date. But we have a commitment towards all the users that if you are still running in your bunker the 0.15 version of Bitcoin Core that nothing bad should happen to you if you follow the most proof of work within the rules that you know.

AvW: That could also mean making it compatible with the LOT=true client.

SP: No, as far as the v0.15 node is concerned there is no LOT=true client.

AvW: Do we want to get into all sorts of scenarios? The scenario that I am most concerned about is the LOT=true chain to call it that if there is ever a split will win but only after a while because you get long re-orgs. This gets back to the LOT=true versus LOT=false discussion in the first place.

SP: I can only see that happening with a massive price collapse of Bitcoin itself. If the scenario comes to be where LOT=true starts winning after a delay which requires a big re-org… if it is more likely to win its relative price will go up because it is more likely to win. But because a bigger re-org is more disastrous for Bitcoin the longer the re-org the lower the Bitcoin price. That would be the bad scenario. If there is a 1000 block re-org or more then I think the Bitcoin price will collapse to something very low. We don’t really care whether the LOT=true client wins or not. That doesn’t matter anymore.

AvW: I agree with that. The reason I am not concerned is what I mentioned before, I think these things will be sorted out by futures markets well before it actually happens.

SP: I guess the futures market would predict exactly that. That would not be good. Depending on your confidence in futures markets which for me is not that amazing.

### Block height versus MTP

https://github.com/bitcoin/bitcoin/pull/21377#issuecomment-818758277

SP: We could still talk about this nitty difference between block height versus block time. There was a fiasco but I don’t think it is an interesting difference.

AvW: We might as well mention it.

SP: When we first described the Speedy Trial we assumed everything would be based on block height. There would be a transformation from the way soft forks work right now which is based on these median times to block heights which is conceptually simpler. Later on there was some discussion between the people who were working on that, considering maybe the only Speedy Trial difference should be the activation height and none of the other changes. From the point of the view of the existing codebase it is easier to make the Speedy Trial adjust one parameter which is a minimum activation height versus the change where you change everything into block heights which is a bigger change from the existing code. Even though the end result is easier. A purely block height based approach is easier to understand, easier to explain what it is going to do, when it is going to do it. Some edge cases are also easier. But to stay closer to the existing codebase is easier for reviewers somewhat. The difference is pretty small so I think some people decided on a coin toss and other people I think agreed without the coin toss.

AvW: There are arguments on both sides but they seem to be pretty subtle, pretty nuanced. Like we mentioned Speedy Trial is going to start on the same date anyway so it doesn’t seem to matter that much. At some point some developers were seriously considering deciding through a coin flip using the Bitcoin blockchain for that, picking a block in the near future and seeing if it ends with an even or odd number. I don’t know if that was literally what they did but that would be one way of doing it. I think they did do the coin flip but then after that the champions for both solutions ended up agreeing anyway.

SP: They agreed on the same thing that the coin flip said.

AvW: The main dissenter was Luke Dashjr who feels strongly about using block heights consistently. He also is of the opinion that the community had found consensus on that and that Bitcoin Core developers not using that is going back or breaking community consensus.

SP: That is his perspective. If you look at the person who wrote the original pull request that was purely height based, I think that was Andrew Chow, he closed his own pull request in favor of the mixed solution that we have now. If the person writing the code removes it himself I think that’s pretty clear. From my point of view the people who are putting in the most effort should probably decide when it is something this trivial. I don’t think it matters that much.

AvW: It seems like a minor point to me but clearly not everyone agrees it is a minor point.

SP: That is what bikeshedding is about right? It wouldn’t be bikeshedding if everybody thought it was irrelevant which color the bike shed had.

AvW: Let’s leave the coin flip and the time, block height thing behind us Sjors because I think we covered anything and maybe we shouldn’t dwell on this last point. Is that it? I hope this was clear.

SP: I think we can very briefly still interject one thing that was brought up which is the timewarp attack.

AvW: We didn’t mention that, that is somewhat relevant in this context. An argument against using block time is that it opens the door to timewarp attacks where miners are faking the timestamps on the blocks they mine to pretend it is a different time and date. That way they can for example just skip the signaling period altogether, if they collude in doing that.

SP: That sounds like an enormous amount of effort for no good reason but it is an interesting scenario. We did an episode about the timewarp attack a long time ago, back when I understood it. There is a soft fork proposal to get rid of it that I don’t think anyone objected to but also nobody bothered to actually implement. One way to deal with this hypothetical scenario is if it were to happen then we deploy the soft fork against the timewarp attack first and then we try Taproot activation again.

AvW: The argument against that from someone like Luke is of course you can fix any bug but you can also just not include the bug in the first place.

SP: It is nice to know that miners would be willing to use it. If we know that miners are actually willing to exploit the timewarp attack that is incredibly valuable information. If they have a way to collude and a motivation to use that attack… The cost of that attack would be pretty low, it would be delaying Taproot by a few months but we would have this massive conspiracy unveiled. I think that is a win.

AvW: The way Luke sees it is that there was already consensus on all sorts of things, using BIP 8 and this LOT=true thing, he saw this as somewhat of a consensus effort. Using block times is frustrating that in his opinion. I don’t want to speak for him but if I am trying to channel Luke a little bit or explain his perspective that would be it. In his view consensus was already forming and now it is a different path.

SP: I don’t think this new approach blocks any of the LOT=true stuff that much. We went through all the scenarios here and the confusion wasn’t around block height versus time, it was on all sorts of things that could go wrong depending on how things evolved. But not that particular issue. As for consensus, consensus is in the eye of the beholder. I would say if multiple people disagree then there isn’t consensus.

AvW: That would also work the other way around. Where Luke disagrees using block time.

SP: But he cannot say there was consensus on something. If people disagree by definition there wasn’t consensus.

AvW: It was my impression that there was no consensus because people disagree. Let’s wrap up. For our listeners that are confused and worried I am going to emphasize the next 3 months Speedy Trial is going to run on both clients. If miners activate through Speedy Trial we are going to have Taproot in November and everyone is going to be happy. We will continue the soft fork discussion with the next soft fork.

SP: We’ll have the same arguments all over again because we’ve learnt absolutely nothing.

-->

### Locked in

![Ep. 40 {l0pt}](qr/40.png)

discuss the lock-in of the Taproot soft fork upgrade.

As discussed in previous episodes, Taproot is a Bitcoin protocol upgrade that will make smart contracts more compact, private and flexible. Aaron and Sjors also discussed the Taproot upgrade process in prior episodes, including the Speedy Trial activation method adopted by Bitcoin Core.

About a week ago, the Speedy Trial signaling threshold was reached, which means Taproot is locked in and will activate later this year. Aaron and Sjors go into further detail about what this means exactly, and what needs to happen before Taproot can ultimately be used on the Bitcoin network safely. Sjors also explains how upcoming Bitcoin Core releases will handle the Taproot upgrade, and what the Bitcoin Core wallet software will and will not enable, while also touching on potential use-cases enabled by the upgrade.

Finally, Aaron and Sjors discuss the Speedy Trial activation process itself, and in particular the lessons learned by it, which could in turn inform future soft fork upgrades. They also briefly speculate which protocol upgrades may be next in line.