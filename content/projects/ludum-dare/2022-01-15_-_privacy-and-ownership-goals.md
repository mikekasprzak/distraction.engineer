+++
title = "Privacy and ownership goals"
#date = "2022-01-15"
draft = true
+++
We do a terrible job of adhering to the GDPR.

Part of that was because we _had_ zero budget. The other part is I didn't really appreciate privacy. I mean, I still use a Gmail account.

Privacy and security go hand in hand, but so does ownership. We've never claimed ownership of anything created during our events, and there's never been a need to. That said, we can't operate unless we have permission to use, showcase, and promote the works submitted to our events.

I've never had a problem with people requesting removal, we're just not technically able to do it right now.

## Adhering to the GDPR

When we switched from WordPress to our custom website code, GDPR requests were few and far between. I can't say we never got them, but I don't remember getting any before the new website. As weird as it sounds, deleting content was not something I planned for. It was on the TODO list, but it wasn't a priority. Today I seem to get a new request every week.

It's a problem. We can to do better.

Deleting data is not _inherently_ difficult. Most of our data is stored in a tree, so we just have to walk the tree and clean up the leaves. The problem is that elements of Ludum Dare were also created in a crunch. I don't feel comfortable deleting content until we've checked all the tables to be sure. Some areas of concern:

* Results: if a winning game removes themselves, we can purge their data, but there's no longer a 4th place game. That would look strange to anyone reviewing previous results.
* Comments: if a post is deleted, we need to do something with the orphaned comments. On other websites it would be fine to just delete them, but comments and _the likes_ on comments affect magic numbers we use to prioritize games. If you left good feedback, should you be penalized if the recipient deletes themselves?

Ultimately I don't think the solution is to delete a users content, but to scrub it: delete the contents, keep but disown the references. We create a special "orphan" user to own all of the orphaned posts, games, and comments. Unfortunately our systems then need to know how to handle the orphan user.

It's not perfect. Technically comments and posts from other users can still make reference to orphaned content. Links from off-site are out of our control, and to change the posts and comments belonging to others would be censorship. Yeah naw, we're not going there, but we can scrub anything you authored.

Unfortunately this isn't something we can half-ass. We (I) need to sit down, review what's there, and build-out support for orphans. I'm not looking forward to it, but it needs to get done.


## Exporting your data

Deleting and exporting are somewhat related. Both need to find all your content, they just have different uses for it. I would like to add a button that would spit out a mammoth JSON file, but I'm concerned this could be abused to slow down the website (we naively cache pages _after_ generating them).

Unlike GDPR, we haven't received any requests for this, so it may be safest to generates this upon request.


## COPPA: Protecting children's privacy by forfeiting yours

I wish GDPR was the only privacy issue, but we also have to worry about COPPA and similar laws about underage users.

I'm not going to go too deep on this, but it _angers_ me that COPPA essentially forces us to ask people their age. Age has no bearing on what we do. Experience maybe, but not age. We haven't done much here either, but I'm disappointed we have to.


## Cookies

I forget the exact wording, but it's my understanding that since we _don't_ use tracking cookies, we don't need a cookie notice.

However, I do need to research "cookie law" and OAuth2. It's a sign-in mechanism where _technically_ cookies follow you across websites, though it is slightly different because you have to opt-in (login) to get them.


## Google Fonts

Some bot looking for a consulting job pointed out that our Privacy Policy doesn't include Google Fonts. When I find a moment, I'll move them to our own servers.


## The Mailing List

During Ludum Dare 49 we officially switched to the new mailing list powered by Revue (a subsidiary of Twitter). I didn't get many complaints, just one, but it was enough to get me thinking. Twitter is free, like Google, because _you_ are the product. Crap.

Before switching to Revue, I hadn't thought much about mailing lists. I also learned Revue wasn't that mature of a service, as I quickly found 2 frustrating (for me) bugs. I like the price (free), but I'm definitely considering my options.

We're already paying for Postmark. We switched to them when I became tired of running my own mail server. _Normal businesses_ seem to use services like Postmark for email lists, so it's worth exploring. Doing so would push us into a higher pricing tier, a tier we wouldn't take advantage of 6 months of the year, but we could integrate it better than Revue. Hmm.


## Discord vs Matrix

The lesser talked about _Google_ in the room is Discord: it's free, so if you use it you're the product. I doubt I'll have luck convincing folks to quit Discord, but we _can_ recommend and provide alternatives. Matrix is a distributed chat network with support for end-to-end encryption. Messages can be federated to any channel on the network. In a way it's like the successor to IRC. On the other hand, with clients like Element, it's nearly indistinguishable from Discord: audio chat rooms and all. To top it off, Matrix can bridge networks, allowing conversations with users on Discord, IRC, Gitter, and others. The downside is you're giving up encryption to support those.

I spun up a [Matrix instance](https://jammer.chat) some months ago. I like it. It's not _safe_ for me to be in a chatroom (easily distracted), but as I grow my team, chat rooms and chat support is something we should have.

That said, I would rather not open-it-up as is. Matrix supports OAuth2, meaning you can use a 3rd party service for authenticating users. Somewhere down the road, I would like to make Ludum Dare accounts compatible with Matrix. Then anyone with a Ludum Dare account will be able to join our server with their username.


## IndieWeb, Federation, and Feeds (RSS)

Going in a different direction, I've starting evaluating some of the efforts of the IndieWeb project. The IndieWeb/`rel="author"`/`rel="me"` standards let us attribute content to you in ways others software can understand. 

Mastodon is a Twitter-like social network. Short messages are broadcast to your followers, but unlike Twitter, those messages are distributed to a federation (open network) of servers.

Data on Ludum Dare could be federated, and I would like to explore that. If nothing else we could create RSS feeds for blog posts and games. Our backend generates proprietary JSON responses, but we could absolutely expand that to include other standards.

This isn't a priority, but this is something I'm exploring.

That what's on my mind. The only priority right now is the GDPR, but I'd like us to dive into the other items in the future.

