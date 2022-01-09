+++
title = "How Jekyll, Hugo, and Zola helped me break-up with C++"
#date = 2022-01-07
[taxonomies]
#tags = ["rust","zola","jamstack","ludum-dare","jammer"]
+++
I've been blogging and journaling [for decades](https://toonormal.com). Still, I regret [not writing more](/blog/blogging-the-new-old-habit/), after years of succumbing to social media.

The way I plan, the way I work, writing makes it better. Going though the motions of committing ideas to page tests them, validates them, and after editing strengthens them. Every project supported by writing is better for it.

My blogging and journaling didn't start as a development problem, but it became one. To me, writing is an incredibly valuable tool, but I demanded better. Something that matched where I was at with programming and web technology. What I didn't expect is that the journey would break me.

When 2021 began, I was a C++ programmer.

For 20 years I was a C++ programmer. No matter what I was making, no matter the language, I was a C++ programmer. Whether I was writing GameBoy assembly code, or backend PHP website code, I would eventually return to the _best_ language of all: C++.

Okay, maybe not _"best"_, but in my mind "C++ with a C style" was the least-worst of the languages. I was never good at articulating _why_ I disliked other languages, but I think I can do a better here.

When 2021 ended, I wasn't a C++ programmer.

Here's my story how searching for a better way to blog broke me.


## Preamble: Static hosting, it's all in the deployment

GitHub has a product called "GitHub Pages" that publishes files from a repository as a static webpage. Obviously GitHub didn't invent static webpages, but I'd say GitHub's ubiquity lead to what happened. 

Static webpage hosting works as you'd expect it: upload some files and it shows up as-is. Static webpages are fast, efficient, and predictable.

How is it different from FTP'ing to an Apache server, like we did 20 years ago?

Generally speaking it's not that different, but workflows today are more refined. Files are stored in source control (GIT), with a full history of changes, commentary, and ticketing available (GitHub). While you can't run server-side scripts, you can customize the deployment process. Standardized tools can take a bunch of files and turn them into a website with consistent styles and structure. Test locally, push a change, and it's live a few minutes later.

Out of the box, GitHub Pages support the Jekyll static webpage generator. Jekyll is a Ruby tool that converts markdown files into HTML files. This is just one of many tools that do this, but if you use GitHub, it might be your first.

You can deploy to GitHub with tools other than Jekyll using "GitHub environments". They're a bit complicated to setup, so I leave that as an exercise to the reader. Alternatively you can create a 2nd repository, or a GIT submodule to store the output from your static webpage generator of choice.

There's a problem though: You should only use GitHub Pages for non-commercial work.

Companies are free to use GitHub Pages, but if you drive a lot of traffic to something hosted on GitHub Pages, I'd worry it could be a problem.

Alternatively you can use a GitHub action to trigger an external service to pull the sources and rebuild. This is fine, but those other services are often not free, and "cost" is something I care a lot about.

A better alternative is CloudFlare Pages. CloudFlare Pages is a free static hosting service in the same vein as GitHub Pages, but you _CAN_ host commercial pages. They also greatly simply the workflow, no need to use GitHub environments and actions. You store your sources on GitHub, and CloudFlare Pages automatically deploys the changes as they are pushed to GitHub. You might also see this called [Jam Stacks](https://jamstack.org/what-is-jamstack/).

TL;DR: It's great. I love a good workflow.

But let's talk about the not-so-good workflows experienced on my journey.


## The Jekyll era (Ruby, Gem)

I ran a WordPress blog for over a decade (and a social network, `*cough*` Ludum Dare). By the end I was tired of, maybe hostile of WordPress.

Blogs are static.

They sometimes change, but never minute to minute, second to second. Why am I generating my blog for every request? This seems incredibly wasteful.

My other problem was I was styling my blogs with HTML. I don't hate HTML, but as a native document format it's clumsy. I'd been using GitHub for years, and through it I became a fan of markdown. It's rare that I need the flexibility that raw HTML provides me.

In early 2018 I migrated my WordPress blog to Jekyll.

The migration wasn't perfect. All of my images and custom styles were now wrong, but my words were there. It was a blog again.

For a while I was content with Jekyll, but the workflow was clumsy and forgettable. That bit me. I took a break from blogging, but came back with no idea how to update my blog. There was a minor but extremely important setup step I could no longer remember, and I never found it mentioned in documentation or my own notes. I could no longer write and test locally. The only way I could blog would be to write, push changes, and hope GitHub Pages wouldn't choke.

This is when I started becoming aware of what I hated about modern programming languages.

Modern programming languages have environments, and these environments require package managers. When you want a JavaScript application, you need NodeJS and NPM. When you want a Python application, to a lesser degree you need PIP. Ruby is no different: You're not getting anywhere without Gem.

Package managers are chains of trust. When you require a package, you're not only depending on a package, you're trusting that package. Packages often depend on other packages, so to trust one package is to implicitly trust several other packages. Yes, I have trust issues, but those issues are well founded. They come from a lifetime of programming, more than half as a professional.

Trust is something I'm going to have to work on, but to my knowledge _trust_ was never a core tenant of Ruby.

The other problem is that every package manager is unique. They're similar, but never the same. The developers of Jekyll may say it's _"easy"_, but package manager powered project comes with baggage: you need to learn the package manager, sometimes the language, to make the most of it.

In my experience, working with Jekyll/Ruby/GitHub pages was incredibly unreliable. GitHub Pages in fine, but I'd make a change to Jekyll files, push it, and it would silently error. I don't doubt some people have had a positive experience with this workflow, but I came to hate updating my Jekyll blog thanks to it.

My blog was stagnant.

## The Hugo era (Go)

I wasn't blogging anymore. My fight with Jekyll took all the joy out of it.

New plan.

Many static webpage generators support markdown. In theory, if I could find an alternative I liked, migrating shouldn't be _that_ much trouble.

I began searching for a Jekyll alternative.

The alternative I settled on was Hugo. Coming from WordPress, I missed editing my blogs from the browser. Early on I found a Hugo plugin called _Hugo Web Admin_ that gives you a web interface for blogging. It was at this time I figured out the "double GitHub repository" trick, where one repo is the sources and a 2nd (a submodule) is the actual GitHub Pages webpage. I would write my posts, locally run a Hugo build, and push both my sources and the submodule.

This setup worked a lot better for me, but _Hugo Web Admin_ wasn't the best tool. Also by design (or my ignorance) Hugo required me to specify a few too many details, including what style to use. If I didn't specify the style, it wouldn't look like my blog.

Jekyll and Hugo are established monolithic projects. The last thing I wanted to do was take the time to _master_ either of them. I incorrectly assumed I could approach blogging with a static website generator _half assed_, like WordPress. WordPress is a bigger, far more complicated project than Jekyll and Hugo combined. WordPress is a complete product, designed to hide everything from the user. Other than JPEG's, the average WordPress user never has to think about files.

Static website generators require strong filing skills.

At a fundamental level, you need to know how to manage files. You should also understand how files and serving files works. You need to go in fully aware that your markdown files are being transformed into one or more HTML files, and that your input file structure may look nothing like your output file structure.

Is that hard? No not really, but it's not _WordPress easy_.

My big mistake diving into static webpage generators was assuming that I wouldn't have to learn them. On the contrary, choosing a static webpage generator is as much of a commitment as anything. It's a powerful tool you can wield, and if you don't respect it, it'll cut you. For safety scissors, stick to a WordPress-like.

I wasn't committed to learning Hugo and Go, and in time I hit the same "how do I update" problem. Also the "double GitHub repository" trick is clumsy, because GIT submodules are clumsy. With Jekyll I could write, push, cross my fingers and hope it worked; But without a working local Hugo setup, I couldn't update at all.

Again, my blog was stagnant.

## The Zola era (Rust, Cargo)

Fast forward to the summer of 2021... **HOLY SHIT!**

My business is thriving (by the way I started a business)! Contracts are signed, time is short, and I have webpages to make! 

**AHHHHH!**

A shame none of this is "blogged" anywhere, but after a year I was still in limbo. My records of this time are a few disjointed journals, video scripts, forum posts, and drafts that later became posts on [ldjam.com](https://ldjam.com). I'd either been unmotivated or too busy to solve my blogging problem, but as the ink dried I needed a static website generator.

Weeks earlier I finished a proof-of-concept for [Jammer.tv](https://new.jammer.tv/) built with CloudFlare Pages and workers. If nothing else I knew CloudFlare Pages could host it. However my experience with Jekyll and Hugo left me ambivalent. I needed a static webpage generator, but which one?

I fired up the CloudFlare Pages documentation and browsed. What _other_ tools do you support? Maybe one of them will catch my eye.

On the bottom of the list was Zola, a lightweight, static webpage generate written in Rust. I liked what I saw in Zola. Fast generator performance, and a _short_ feature list. Less features meant it could be mastered in less time.

Bottom of the list though. That shouldn't inspire much confidence, but it _was_ mature enough for CloudFlare to support and document it.

I don't have a better plan, so okay, let's try Zola.

I'd actually been dabbling with Rust for a while, having made my way through [The Rust Book](https://doc.rust-lang.org/stable/book/) some months earlier. I was still very suspicious of package managers, though I couldn't yet articulate why (trust). Given my choices, Rust seemed the most likely language I'd eventually use.

Over a long day I pulled together the [Interactive Snacks](https://interactivesnacks.com) website. I started with one of the templates, and hacked it into something that met my needs. This was a simple website with pages, no blog. It was a hack-job and little gross, but it worked for me.

I went deeper.

After combing over the docs for a few days, I had a pretty good idea of everything Zola could do. Again it's not the most featured static webpage generator, but if I stay in my lane, I knew exactly what it could do and how to do it. I tackled the first draft of [Ludum Dare's professional website](https://ludumdare.com). This website wasn't a blog, but it could be built with multiple blog-like feeds (folders).

* A news/press releases feed
* A schedule feed
* A sponsors feed
* A games feed (currently disabled)

Having taken the time to ship a Zola project first (Interactive Snacks website), to then learn Zola in depth, before making and executing my plan really helped it succeed. I am confident Zola will be _enough_ to handle both websites, more websites (this blog and the Academy), and I'm confident I can teach it to others.

The workflow is straightforward and memorable... if you know Cargo.

I could continue singing the praises of Zola, but I have to admit I didn't give Jekyll and Hugo an equal shake. I'm not interested in investing my time in Ruby or Go. If Ruby or Go fill other needs for you, then Jekyll and Hugo might fill your need for a static website generator. 

My takeaway is you should choose a static website generator that lives in the same head-space as you.

With that out of the way, Zola really was the best option (for me). 

Circling back to trust and package managers, Rust is pedantic. As far as general programming languages go, Rust is so much more aggressive about being syntactically correct than any language I've used. You can still mess up an algorithm, but you shouldn't mess up a bounds check. A case could be made for Haskell, but I wouldn't write an OS in Haskell (not to mention some of the templating stuff gives me "ugly PHP namespaces" vibes).

Do I trust Cargo packages implicitly? No of course not, but I must admit the Rust compiler holds code quality to a higher standard than I do (sometimes). It took time and using, but I trust it enough to use it.


## Who are you?

My identity as a "C++ programmer" is definitely broken, and I just spent several paragraphs singing the praises of Rust.

Am I a Rust programmer?

No, not yet, but I would say I'm a Rust enthusiast.

My identity of the last two decades was tied to what I saw as _the best_ programming language, but I struggled to find anything better. Other than JavaScript, every other language I touched let me down. And since my Rust "epiphany", I don't look forward to touching C++ anymore.

At best I would identify as a programmer, but I can't decide if that's me anymore. If you asked what my job was, sure, I'd say programmer. But who am I? What is your modus operandi?

Mike, the 41 year old kid who got a senior game programming job at 19. Mike who is running his second company, who spends most his time _not_ making games yet identifying as a game developer. Who are you?

Identity is complicated.

Lets talk about that next time.