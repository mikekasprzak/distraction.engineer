+++
title = "Static websites, and my journey with Jekyll, Hugo, and Zola"
#date = 2022-01-07
[taxonomies]
#tags = ["rust","zola","jamstack","ludum-dare","jammer"]
+++
In mid-late 2021, I was writing a video tutorial on hosting web games on GitHub. Along the way, I found myself researching CloudFlare pages. CF pages is a free static hosting service in the same vein as GitHub pages.

Generally speaking, static hosting works as you'd expect it: upload some files and it shows up as-is. Static pages are fast, efficient, and predictable. Their one caveat though is you can't use any server-side scripting.

How is this different from an Apache server and an FTP account from 20 years ago? Other than the cost (free), while you can't run server-side scripts, you can run a build script. That is where the magic happens.

Out of the box, GitHub pages support the Jekyll static website generator. Jekyll is a Ruby tool that converts markdown files into HTML files. This is hardly the only tool that does this, but if you GitHub, it might be your first.

You can use tools other than Jekyll on GitHub, but it's complicated. GitHub supports something called environments. You can setup an environment that takes the sources from one repository, run them through a static website generator, and push them to a 2nd repository. Cool. It's a bit hacky, but that is a solution.

There's a problem though: GitHub pages are for non-commercial work.

Alternatively you can use a GitHub action to trigger an external service to pull the sources and rebuild. This is fine, but those other services are often not free.

CloudFlare pages by comparison let you host commercial pages, for free, and to their credit they greatly simply the workflow.

What is _the web_ without buzz words? To contrast to greatest (most painful) buzzword in web technology: _"full stack"_, Cloudflare calls what they do a [JamStack](https://jamstack.org/what-is-jamstack/). I die a little when I discover a new buzzword, but I legit do think JamStacks are great. JamStack's aren't exclusively a CloudFlare innovation, but CF offers such a great service I expect they'll monopolize the space.


## I hate Jekyll (Ruby, Gem)

I had run a WordPress blog for over a decade, and I was tired of it. Blogs are static. Why am I generated pages for something static? That seemed incredibly inefficient.

The other problem was I was styling by blogs with HTML. I don't hate HTML, but as a native document format it's clumsy. I'd been using GitHub for years, and I was a fan of markdown. It was rare that I needed the flexibility raw HTML provided me, and I was more than happy using markdown.

So, one day, I decided to migrate my WordPress blog to Jekyll.

The migration wasn't perfect, but my written content was now markdown. Many of my links and styles broke, but haha, no big deal, I'll fix that later... famous last words.

For a while I was content with Jekyll, but it was clumsy. I took a break from blogging, but came back with no idea how to update my blog.

Modern programming languages have environments, and those environments often include package managers. When you want a JavaScript application, you install NodeJS; When you want a Python application, you get PIP; Ruby is no different, you need Gem.

The problem is every language and package manager is unique. They're similar, but never the same. The developers of Jekyll may say it's _"easy"_, but it comes with baggage: you need to learn Ruby and Gem.

In my experience, working with Jekyll/Ruby was incredibly unreliable. I'd make a change, and it wouldn't stick, it'd occasionally crash, or it would take a really long time to build. I don't doubt some people had a positive experience with Ruby, but I came to hate updating my Jekyll blog. I had zero interest in Ruby as a language, and I was going to have to teach myself all about its toolchain and build environment to understand why it kept messing up.

No thanks.


## I don't like Hugo (Go)

Due to my fight with Jekyll, I wasn't blogging anymore. Many static website generators support markdown, so I figured I could migrate to an alternative without much trouble, so I began the search.

A popular option was Hugo, so I dug into it. Coming from WordPress, I missed being able to edit my blogs from the browser. Early on I found a Hugo plugin called _Hugo Web Admin_ that gives you something like a web interface for blogging. Cool! So we migrated to Hugo.

Hugo worked a lot better for me, but _Hugo Web Admin_ wasn't the best tool. Also by design (or my ignorance) Hugo required me to specify a few too many details in the front-matter, including what style to use. If I didn't specify the style, it wouldn't look like my blog.

This fed back into my criticism of Jekyll: to learn how to use this better, I was going to have to learn Go. For a while I did dabble with Go as a programming language, but it didn't feel different enough. I often criticize C# for being a crippled C++ with a newer core library. Go as a language seemed _fine_, but it wasn't radically different enough to justify the effort. I wouldn't gain much teaching myself Go.

Like before, my blogging became stagnant, once I forgot how to use and setup the tools.


## I like Zola (Rust, Cargo)

Fast forward to mid-2021. Holy shit! My business is thriving! Contracts are signed, and I had to pull some websites out-of-my-butt.

I'd just finished a proof-of-concept for the [Jammer.tv](https://new.jammer.tv/) built on CloudFlare pages and workers, so I knew pages was an option here. However, my experience with Jekyll and Hugo left me ambivalent. I knew I wanted a static website generator, but the ones I'd used in the past left something to be desired.

So I fired up CloudFlare pages and browsed. What else do you support? Maybe one of them will catch my eye.

At the bottom of the list was Zola, a lightweight static website generate I'd never heard of, written in Rust.

I'd been dabbling in Rust, having made my way through [The Rust Book](https://doc.rust-lang.org/stable/book/) some months earlier, but I was still very suspect of package managers. Still, I liked what I saw in Zola. Fast generator performance, and a short feature list. Less features might sound like a downside, but it meant I could "master" it in less time.

So okay, let's Zola.

In a couple days I pulled together the [Interactive Snacks](https://interactivesnacks.com) website. I started with one of the templates, and hacked it into something that met my needs. This was a simple website with pages, no blog. Cool, that worked out.

I dove deeper.

After combing over the docs for a few days, I had a pretty good idea what I could pull off with Zola. Again it's not the most featured static website generator, but if I stay in my lane, I knew exactly what it could do and how to do it. I tackled the first draft of [Ludum Dare's professional website](https://ludumdare.com). This website wasn't a blog, but I decided it could be built with multiple blog-like feeds.

* A news/press releases feed
* A schedule feed
* A sponsors feed
* And eventually, a games feed

This was my first serious use of Zola, and it worked out great. The website as it stands today needs some work, but I'm confident that the tooling will handle years of growth, and that I'll be able to teach others how to update it.

That does bring up my concern from before: to make the most of Zola, I would have to learn Rust, specifically Cargo the package manager. That meant learning `TOML`, and learning to trust a package manager. I didn't immediately trust it, that's something that happened months after, as I continued to explore Rust as a programming language.

Rust has a lot going for it. It approaches programming in a radically different way compared to C++, C#, Ruby, Go, and others. I used to think I wanted a language that was somehow more suited to game making (see Jonathan Blow's JAI), but what I found instead was a language that challenged my preconceptions, and promised me safer code if I followed the rules. This was the "radically different" from C++ I was looking for.

I use Zola now. This blog is my latest Zola project, and expect it wont be my last. 

My Zola journey is a journey in Rust as well. This wasn't going to work if I didn't commit to it and the package manager. In time, Rust helped me trust a package manager, which was hard given the dependency hells I grew up with (C, C++, Win32, etc). 

I'm excited to do more with both Zola and Rust.