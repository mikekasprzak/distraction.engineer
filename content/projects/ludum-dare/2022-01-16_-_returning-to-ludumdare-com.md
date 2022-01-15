+++
title = "Returning to ludumdare.com"
#date = "2022-01-15"
draft = true
[taxonomies]
tags = ["ludum-dare","jammer","interactive-snacks"]
+++
While [writing about privacy](/projects/ludum-dare/privacy-and-ownership-goals/), I found myself thinking about user support. Having a support-database is pretty common, and I wasn't super thrilled about creating one on FreshDesk. For one it's _yet another_ series of accounts, validations, and pages I would have to manage. Bringing people in to help would be a pain. It would be better if we could build a support database using something we already use.

I eventually realized a support database is just a static-website, like this blog. Hey! Zola would be perfect for building a support database! Not only does it display content, but once the data is on GitHub, **ANYONE** can contribute! Now that's more like.

Okay, lets create a support website. Where does it go, and what's the URL?

Right now the FreshDesk support portal is on [support.ldjam.com](https://support.ldjam.com). It's a CNAME alias to the actual support website on FreshDesk. It's not actually something we own.

Okay, we could replace `support.ldjam.com` with a Zola website. Cool.

What goes on the website? What are our frequently asked questions?

Ah... right... rules questions.

Okay, we could move the rules page to `support.ldjam.com`. Side benefit: users could contribute fixed to the rules page. Awesome!

But, should Ludum Dare's rules be buried on a support website? For that matter, should frequently asked questions?


## ludumdare.com and ldjam.com

The reason we use `ldjam.com` for events is because at the time I couldn't figure out a nice way to make the two websites coexist on the same domain. Frankly the legacy website `ludumdare.com/compo/` shouldn't exist, but until that data is migrated it needs to be there.

`ludumdare.com` today is a bit of a hack. It's two websites, a simple static website `ludumdare.com` built with Zola, and the decrepit remains of the WordPress blog at `ludumdare.com/compo/`. Until yesterday I didn't even have the websites configured correctly (`ludumdare.com/compo/` wasn't browsable).

As things are today, I don't like how `ludumdare.com` works. In essence it's the _legacy_ server running and hosting `ludumdare.com`. That's fine since the legacy website isn't being used, but I need to reboot and maintain that machine more often then I'd like. The last thing I want is _broken WordPress_ to cripple `ludumdare.com`, especially since static websites are _prime_ candidates to be hosted on CDN edge nodes. No computations required. Just raw files flowing, sitting in caches for optimal performance.

My personal goal is to be free of `ludumdare.com/compo/` before the end of the year. That means me and employee, whomever you are, we figure out how and what to extract from WordPress, import it, and finally sunset the `/compo/` website by next April's event.

So alright. If the `/compo/` website is finally going away, we can start talking about overhauling _all_ URLs.


## Today's ludumdare.com

`ludumdare.com` is the homepage. We've never done much with our homepages, we ran `ludumdare.com/compo/` and `ldjam.com` instead. We should though. I threw together the [`ludumdare.com`](https://ludumdare.com) you see today _basically_ to make sponsors happy. They didn't exactly require it, but I thought it was important to have one just to seem professional.

What `ludumdare.com` should do is get you excited while quickly explain what we are, show you what you need to do, and where you need to go. Currently there's a banner, a schedule, and a log of previous news posts.

It can be better.

I want a feed of commercialized Ludum Dare games on the homepage, just like we have with the (ahem) neglected Steam Curator page. Why? Because it's cool! And because our source is on GitHub, users could contribute games to the list with a pull request. If done correctly, that's a one click approval for me (merge). Easy!

If we move the rules to `ludumdare.com/rules/`, not only is that URL sensible, it means anyone can contribute to the rules page. If we need a clarification, if I made a typo, whatever the case. And again, if done correctly, it's a one click approval from me (merge). No need to write me an email explaining the problem, you can just fix it.

(Note to self: consider adding some styles to the page that link directly to their sources on github, to make it more obvious anyone can edit)

Finally if we setup `ludumdare.com/faq/` and `ludumdare.com/support/` to recommend searching the faq, we can have a _not shit_ way to find answers to questions, before final escalation to me. And same deal, it being on GitHub means anyone can contribute frequently asked questions as issues, or answers as pull requests.

And hooray, Ludum Dare support will sucks less.


## The event database

I'll think of a better name for it later, but once the rules are extracted from `ldjam.com`, we can start thinking about the code and database ase their own entities. Obviously the database is useless without the code, but there's a lot we cram into the experience of `ldjam.com`.

My intention was to build a general game jamming website and database, but I didn't have the resources or experience to pull it off. `ldjam.com` was going to be the "Ludum Dare" version of the data, while `jammer.vg` would be the general view. I have legacy data from not only previous Ludum Dare events, but several other jams that came and went. It's a shame seeing things disappear without a trace, and I wanted our efforts to help preserve, like a [Mobygames](https://mobygames.com) of jamming.

I've also had numerous companies approach me about running events on their behalf. If we could unify the data, let anyone with an existing account _just use it_, that would make those events more appealing to everyone.


## jam.ludumdare.com

As I went back and forth trying to decide how to structure everything, especially once we've migrated the old data, I've come to the conclusion that `ldjam.com`'s days are numbered.

For the sake of not breaking the internet, the domain will still exist. We'll need a thing for `ludumdare.com/compo/` as well, but soon they will exist solely to make old links point to their new home. I haven't decided _how_ to do this, but I have been fond of static websites lately.

A sensible way to structure things would be to put the _static stuff_ on `ludumdare.com`, and then make the event website a subdomain: `jam.ludumdare.com`.

I think this is the goal.

