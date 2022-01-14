+++
title = "Picking up where 2021 left off"
#date = 2022-01-14
[taxonomies]
tags = ["ludum-dare","jammer","interactive-snacks","business","privacy","security"]
+++
Ludum Dare 50 is on April 1st. There are 11 weeks until Ludum Dare 50.

We've signed one sponsor so far, our first _integration_ sponsorship. I'll talk more about specifics later, but this is new and will lead to some really cool stuff in the future. I need to reach out to our previous sponsors, see who wants in for LD50, as well as open the invitation to others. Admittedly we're still new very new to sponsors. We don't have a pitch deck (yet). Every deal is worked out with the sponsor directly. I'm also still learning what sponsors want.

Last year I failed to hire someone. We had some great candidates apply, but they weren't what I needed. To be fair, I need a someone able to architect, clean up, and essentially take-over for me, but I am constrained by my budget. I also wasn't offering anything _sexy_ in the work; PHP and JavaScript just don't impress like they used to.

I need to revisit this. Make the gig less-demanding, sexier, or more widely available. Until then, it looks like Ludum Dare 50's tech is on me.

It was a blessing in disguise though. I underestimated our costs, and I didn't appreciate _when_ we would be paid. Taking sponsors for Ludum Dare 49 was my introduction to _payment terms_. This is a standard practice where you give a _term_ when payment is due. Net 30 meaning payment within 30 days. I was also too relaxed about chasing for late payments, though their lateness was more my fault. But, by year end everything was sorted.

`*phew*`

Short term task: we need an invitation banner! It's Ludum Dare's 20th anniversary, so I decided to go with a birthday motif. I picked up some party supplies, candles, I just need a cake (and _oh darn_, then I'll have to eat the cake). 

As far as tech goes, there are 3 big things that need to happen.

1. Finish migrating the servers.
2. Switch images to bucket storage.
3. Add support for Encore events (and explain what they are).

To clarify, the migration task is our _current_ servers, not the legacy WordPress server. With newer servers we get improved security, privacy, flexibility, and performance.

Security and privacy are things I've become a lot more conscious of this past year. I wont pretend I'm an expert, but I'm far more paranoid today than I've even been. We can do better, and we will.

Flexibility, I suppose we're following the natural progression of things. Legacy (WordPress) Ludum Dare was single server with everything on it. Current Ludum Dare separated the database from the frontend and other services. The next generation will further split things, more standalone servers, replication, and a node balancer in front so we can build microservices behind it. The migration has been sitting in a half-done state for a few years now, and it's time I finished it.

The 2nd task is storing image uploads in a bucket (S3-like). We currently store all image on an addon hard-drive attached to one of the servers. This works fine, but every couple events we need to upgrade the hard drive. Also since we did this, the price of _bucket_ storage has dropped dramatically: it's now 1/4th the price! You don't even have to manage a server anymore with buckets! If needed we could delay this change, but it's going to save us money so I'd like to do it.


## Ludum Dare Encore

This is the probably the most significant task as it changes how people Ludum Dare. Encore events are the new 3rd category of Ludum Dare submissions. In essence it's the same as the Jam category, but there is no time limit (†) and no winners. Okay, I say no time limit, but realistically you want to finish your game before results day. If you do, you get added to the same feedback pool as everyone else. If you get enough ratings, you'll still get an average, but you wont be scored against other games.

We need to do work on scoring. Calculating scores and other metrics like _coolness_ is a very expensive process right now. That's part of the reason we only deal with one event at a time, and are somewhat vague about how it works. We can do better than averages too. Down the road, I would like Encore games to be given some indication how you did compared to other games.

A side benefit of the Encore system is it _should_ increase engagement during the entire rating period. How much is still to be determined.

Encore games will _eventually_ expand and let you submit for any previous Ludum Dare theme, not by Ludum Dare 50 though. I don't yet know how to drive engagement (feedback) to those older games.


## Is that all?

No, there is still the _sponsor integration_ I mentioned, and fixing things as they happen. We have other challenges like spammers, user account changes (i.e. support), and GDPR requests to tackle. I don't know how or when these things will get dealt with, but we'll do what we can.

The last big thing I'd like to do is prioritize tasks and our massive list of GitHub issues. Since I don't have the luxury (budget) to find and hire someone to take-over decision making, I need to do this myself. I'm not super fond of the _Linus Torvalds_ role where _coding_ means spending your days approving commits, but so be it. That's what's needed of me. Find the money, make sponsors happy, delegate the work.

I'll leave things here. First development log finished.