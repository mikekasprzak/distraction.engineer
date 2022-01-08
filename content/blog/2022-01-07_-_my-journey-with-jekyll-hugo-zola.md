+++
title = "Static webpages, and my journey with Jekyll, Hugo, and Zola"
#date = 2022-01-07
[taxonomies]
#tags = ["rust","zola","jamstack","ludum-dare","jammer"]
+++
In mid-late 2021, I was writing a video tutorial on hosting web games on GitHub. GitHub as a product called "GitHub Pages" which lets you use the files in a GitHub repository as a static webpage.

 Many web projects don't need to be dynamic. Today static webpages are a great choice. Static webpage hosting works as you'd expect it: upload some files and it shows up as-is. Static webpages are fast, efficient, and predictable.

How are things different from FTP'ing to an Apache server, like we did 20 years ago?

Generally speaking it's not much different, but workflows today are more refined. Files are stored in source control (GIT), which means we have a fully history of changes, commentary, and ticketing available (GitHub). While you can't run server-side scripts, you can fully customize the deployment process. Standardized tools can take inputs like a bunch of markdown files and turn them into a website with consistent styles and structure. Push a change and it goes live a few minutes later.

Out of the box, GitHub pages support the Jekyll static webpage generator. Jekyll is a Ruby tool that converts markdown files into HTML files. This is hardly the only tool that does this, but if you GitHub, it might be your first.

You can deploy with tools other than Jekyll on GitHub with "environments". They're a bit complicated to setup, so I leave that as an exercise to the reader. Alternatively you can create a 2nd repository, or a Git Submodule to store the output from your static webpage generator of choice. Point being: you have options depending on how much you want to automate the process.

There's a problem though: You can only use GitHub pages for non-commercial work.

Companies are free to use GitHub pages, but if you drive a lot of traffic to something hosted on a GitHub pages, I worry it could be a problem.

Alternatively you can use a GitHub action to trigger an external service to pull the sources and rebuild. This is fine, but those other services are often not free, and "cost" is something I care a lot about.

Along the way I found myself researching CloudFlare pages. CloudFlare pages is a free static hosting service in the same vein as GitHub pages, but you _CAN_ host commercial pages on CloudFlare pages. They also greatly simply the workflow. You store your sources on GitHub, and CloudFlare pages automatically deploys the changes as they are pushed. It's great.

I love a good workflow.

Blogging and journaling, while never a primary activity for me, I think they both greatly improve my process; How I plan. When I can't blog, I don't feel like I'm doing my best. Though blogging didn't start as development problem, it became one. As a somewhat older developer—set in my ways—my journey choosing static webpage generators greatly influenced my take on modern development. Here's my story.

## Jekyll (Ruby, Gem)

I ran a WordPress blog for over a decade (and a social network, `*cough*` Ludum Dare), and by the end I was tired of WordPress. Blogs are static. They sometimes change, but never minute to minute, second to second. Why am I generating my blog on every request? This seemed incredibly inefficient.

My other problem was I was styling by blogs with HTML. I don't hate HTML, but as a native document format it's clumsy. I'd been using GitHub for years, and though it I became a fan of markdown. It was rare that I needed the flexibility raw HTML provided me.

So I decided to migrate my WordPress blog to Jekyll.

The migration wasn't perfect. All of my images and custom styles were now wrong, but the words were there.

For a while I was content with Jekyll, but the workflow was clumsy and forgettable. That bit me. I took a break from blogging, but came back with no idea how to update my blog. There was a minor but extremely important step I could no longer remember. I couldn't test locally. The only way I could update would be to push changes and hope for the best.

I think this was when I first figured out what I hated about modern programming languages.

Modern programming languages have environments, and those environments often include package managers. When you want a JavaScript application, you install NodeJS; When you want a Python application, you get PIP; Ruby is no different, you need Gem.

The problem is every language and package manager is unique. They're similar, but never the same. The developers of Jekyll may say it's _"easy"_, but every package manager powered language comes with baggage: you need to learn it and its package manager.

In my experience, working with Jekyll/Ruby/GitHub pages was incredibly unreliable. GitHub pages in fine, but I'd make a change to Jekyll files, push it, and it silently error. I don't doubt some people have had a positive experience with this workflow, but I came to hate updating my Jekyll blog. I have zero interest in Ruby as a language, and I was going to have to learn it and Gem to make my blog work right.

No thanks.


## Hugo (Go)

Due to my fight with Jekyll, I wasn't blogging anymore. Since many static webpage generators support markdown, I figured I could migrate to an alternative without much trouble, so I began the search.

A popular option was Hugo, so I dug into it. Coming from WordPress, I missed being able to edit my blogs from the browser. Early on I found a Hugo plugin called _Hugo Web Admin_ that gives you something like a web interface for blogging. Cool! Lets try Hugo.

Hugo worked a lot better for me, but _Hugo Web Admin_ wasn't the best tool. Also by design (or my ignorance) Hugo required me to specify a few too many details, including what style to use. If I didn't specify the style, it wouldn't look like my blog.

This feeds back into my general criticism of Jekyll: to learn how to use this better, I was going to have to learn Go and Go's package manager. I did briefly dabble with Go as a programming language, but it didn't feel different enough. I frequently criticize C# for being a crippled C++ with a better core library. Go as a language seemed _fine_, but nearly being a C++11 expert, it wasn't radically different enough. I wasn't going to gain much teaching myself Go.

Like before, my blogging became stagnant from not understanding the tooling.


## Zola (Rust, Cargo)

Fast forward to mid-2021. Holy shit! My business is thriving! Contracts are signed, and I had to pull some websites out-of-my-butt.

I'd just finished a proof-of-concept for the [Jammer.tv](https://new.jammer.tv/) built on CloudFlare pages and workers, so I knew pages was an option here. However, my experience with Jekyll and Hugo left me ambivalent. I knew I wanted a static webpage generator, but the ones I'd used in the past left something to be desired.

So I fired up CloudFlare pages and browsed. What else do you support? Maybe one of them will catch my eye.

At the bottom of the list was Zola, a lightweight static webpage generate written in Rust.

I'd been dabbling in Rust, having made my way through [The Rust Book](https://doc.rust-lang.org/stable/book/) some months earlier, but I was still very suspect of package managers. Still, I liked what I saw in Zola. Fast generator performance, and a short feature list. Less features might sound like a downside, but it meant I could "master" it in less time.

So okay, let's try Zola.

In a couple days I pulled together the [Interactive Snacks](https://interactivesnacks.com) website. I started with one of the templates, and hacked it into something that met my needs. This was a simple website with pages, no blog. Cool, that worked out. It was clumsy and a little gross, but it did the job.

I dove deeper.

After combing over the docs for a few days, I had a pretty good idea of everything Zola could do. Again it's not the most featured static webpage generator, but if I stay in my lane, I knew exactly what it could do and how to do it. I tackled the first draft of [Ludum Dare's professional website](https://ludumdare.com). This website wasn't a blog, but I decided it could be built with multiple blog-like feeds.

* A news/press releases feed
* A schedule feed
* A sponsors feed
* And a games feed (currently disabled)

This was my first serious use of Zola, and because I was able to take the time to learn it, it worked out great. The website as it stands today needs some work, but I'm confident that the tooling will handle years of growth, and that I'll be able to teach others how to update it.

That does bring up the same concerns from before: to make the most of Zola, I would need to learn Rust and the Cargo the package manager. That also meant learning to trust a package manager. I didn't immediately trust it, but as I continued to explore Rust as a language, I warmed up to it.

I use Zola now. This blog is my latest Zola project, and expect it wont be my last. 

My Zola journey is a journey in Rust as well. This wasn't going to work if I didn't commit to it and the package manager. In time, Rust helped me trust a package manager, which was hard given the dependency hells I grew up with (C, C++, Win32, etc). 

I'm excited to do more with both Zola and Rust.