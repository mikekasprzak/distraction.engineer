+++
title = "Privacy and ownership goals"
#date = "2022-01-15"
#draft = true
[taxonomies]
tags = ["ludum-dare","jammer","privacy","security"]
+++
We haven't done a great job with privacy.

Part of that is because nearly everything on the website is public, part was because we _had_ zero budget, and part was because I _didn't_ appreciate privacy. I think I'm getting better, but even I still use a Gmail account.

I've never had a problem with people requesting removal, we're just not technically able to do it right now.

Privacy and security go hand in hand, but so does ownership. I think we've done fine with ownership, but there's always room to grow. We've never claimed ownership of anything created during our events. Frankly there's never been a need to. That said, we can't operate unless we have permission to use, showcase, and promote the works submitted to our events. A Ludum Dare where we can't share doesn't sound very interesting.

Here's a quick look at some privacy and ownership issues, and some of my thoughts on each.

## Adhering to the GDPR

When we switched from WordPress to our custom code, GDPR requests were few and far between. I can't say we never got them, but I can't remember getting any before the switch. As weird as it sounds, deleting content was not something I planned for. It was on the TODO list, but it wasn't a priority. Today I get a new request every week, and it's a problem.

We can to do better.

Deleting data is not _inherently_ difficult. Most of our data is stored in a tree, so we just have to walk the tree and clean up the leaves. The problem is that elements of Ludum Dare were also created in a crunch. I don't feel comfortable deleting content until we've assessed and checked all the tables to be sure. Some areas of concern:

* Results: if a winning game removes themselves, we can purge their data, but there's no longer a 4th place game. That would look strange to anyone reviewing previous results.
* Comments: if a post is deleted, we need to do something with the orphaned comments. On other websites it would be fine to just delete them, but comments and _the likes_ on comments affect magic numbers we use to prioritize games. If you left good feedback, should you be penalized if the recipient deletes themselves?

Ultimately I don't think the solution is to delete a users content, but to scrub it: delete the contents, keep the hierarchy, but disown the references. We create a special "orphan" user to own all of the orphaned posts, games, and comments. Unfortunately our systems then need to know how to handle the orphan user.

It's not perfect. Technically comments and posts from other users can still make reference to orphaned content. Links from off-site are out of our control, and to change the posts and comments belonging to other users would be censorship. I think it's reasonable to limit "deleting" to content you authored.

Unfortunately this isn't something we can half-ass, or put off much longer.


## Exporting your data

Deleting and exporting are somewhat related. Both need to find all your content, they just have different uses for it. I would like to add a button that would spit out a mammoth JSON file, but I'm concerned this could be abused to slow down the website (we naively cache pages _after_ generating them).

Unlike GDPR, we haven't received any requests for this, so in the short term it may be safest for us to generates these upon request.


## COPPA: Protecting children's privacy by forfeiting yours

I wish GDPR was the only privacy issue, but we also have to worry about COPPA and similar laws about underage users.

I'm not going to go too deep on this today, but it _angers_ me that COPPA essentially forces us to ask people their age. Age has no bearing on what we do. Experience maybe, but not age.

Anyway, I need to dig more into this. I would like to make it a simply checkbox on sign-up, "are you 13+", and if not have some way for a guardian to concent on your behalf (i.e. send them an email).

If at all possible, I **don't** want to store your age in your account data, but we'll see.


## Cookies

I forget exactly what it says, but it's my understanding that since we _don't_ use tracking cookies, we don't need a cookie notice.

However for the future, I need to research "cookie law" and OAuth2. OAuth2 is a sign-in mechanism where _technically_ cookies follow you across websites, though it is slightly different because you have to opt-in (login) on every website to get them.


## Google Fonts

Some bot looking for a consulting job pointed out that our Privacy Policy doesn't include Google Fonts. When I find a moment, I'll move them to our own servers.


## The Mailing List

During Ludum Dare 49 we officially switched to the new mailing list powered by Revue (a subsidiary of Twitter). I didn't get many complaints, just one, but it was enough to get me thinking. Twitter is free, like Google, because _you_ are the product.

...okay, fair point.

Before switching to Revue, I hadn't thought much about mailing lists. In hindsight Revue is an immature service. I like the price (free), but I'm definitely considering my options.

We're already paying for Postmark. We switched to them when I became tired of running my own mail server. _Normal businesses_ seem to use services like Postmark for email lists, so it's worth exploring. Doing so would push us into a higher pricing tier, a tier we wouldn't take advantage of 6 months of the year, but we could integrate it better than Revue. Hmm.


## Discord vs Matrix

The lesser talked about _Google_ in the room is Discord: it's free, so if you use it you're the product. 

I doubt I'll have luck convincing folks to quit Discord, but if we provide an alternative, folks may take it under consideration. Matrix is a distributed chat network with support for end-to-end encryption. Not only does nobody own Matrix, but conversations can be kept private. Messages can be sent (federated) to any channel on any server on the network, and encryption is upheld. Like IRC, Matrix is a protocol, not a client. With clients like Element, using Matrix can appear indistinguishable from Discord: audio chat and everything. To top it off, Matrix can bridge to other networks, allowing conversations with users on Discord, IRC, Gitter, and others. The downside of bridges is you give up encryption.

I spun up a [Matrix instance](https://jammer.chat) some months ago. I like it. I try to avoid chat rooms myself, but they're useful when working together.

I'm not planning to open-up the Matrix server _right now_. One of the more popular Matrix server-softwares supports OAuth2, meaning you can use a 3rd party service for authenticating users. Somewhere down the road, I would like to make Ludum Dare accounts compatible with Matrix. Then anyone with a Ludum Dare account will be able to join the server with their Ludum Dare username.


## Support, ticketing, and frequently asked questions

Today you "email Mike", but my email inbox is where messages go to die. I would like this moved entirely over to ticketing.

I spun up a freebie [FreshDesk](https://ludumdare.freshdesk.com) account, forwarded many emails to it, but I'm undecided if this is the solution. Again I have concerns about the privacy of our users, given it's a free service. They claim it's free because they want to up-sell other services, but I'm going to have to read the agreements again and ponder it versus the open source solutions. Adding new servers to maintain is a concern though. There's only one of me, and I need to get things off my plate.

On the plus side, many support requests _could_ be handled with code and education. They're usually things like renaming accounts, email activation issues, or submission concerns. There will always be tasks too niche for features, but many can be solved with them.

We should have a support database, a place for frequently asked questions. FreshDesk includes one, but I don't like how I'll have to manage contributions.

I think my solution will be to start a new community project: a Ludum Dare support website, as a GitHub project. Invite folks to contribute frequently asked questions as issues, or submit pull requests if they want to answer them.

A _feature_ of [ldjam.com](https://ldjam.com) that never went live was patching. Any piece of content, any post, any game, could receive patches from fellow community members. I would still like to see this added to the website some day, but it's a waste being unable to contribute small changes like typos. I'm thinking we should move the rules together with the support database.

I'll come back to this.


## IndieWeb, Federation, and Feeds (RSS)

Going in a different direction, I've starting evaluating some of the efforts of the IndieWeb project. The IndieWeb/`rel="author"`/`rel="me"` standards let us attribute content to you in ways others software can understand. 

Mastodon is a Twitter-like social network. Short messages are broadcast to your followers, but unlike Twitter, those messages are distributed to a federation (open network) of servers.

Data on Ludum Dare could be federated, and I would like to explore that. If nothing else we could create RSS feeds for blog posts and games. Our backend generates proprietary JSON responses, but we could absolutely expand that to include other standards.

This isn't a priority, but this is something I'd like to explore.


## Wrap-up

That what's on my mind. The highest priority is handling the GDPR, but I'd like us to dive into the other items listed in the future.

