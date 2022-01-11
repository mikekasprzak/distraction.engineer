+++
title = "How blogging software helped me break-up with C++"
#date = 2022-01-07
[taxonomies]
tags = ["blogging","rust","zola","wordpress","jamstacks","github","cloudflare","static-web","ludum-dare","jammer","interactive-snacks"]
+++
I've been blogging and journaling [for decades](https://toonormal.com). I do regret [not writing more](/blog/blogging-the-new-old-habit/), after years of succumbing to social media.

The way I plan, the way I work, writing makes it better. Going though the motions of committing ideas to page tests them, validates them, and after editing strengthens them. Every project supported by writing is better for it.

My blogging and journaling didn't start as a development problem, but it became one. To me, writing is an essential tool, and I demanded better. Something that matched where I was at with programming and web technology. What I didn't expect is that the journey would break me.

When 2021 began, I was a C++ programmer.

For 20 years I was a C++ programmer. No matter what I was making, no matter the language, I was a C++ programmer. Whether I was writing GameBoy assembly code, or backend PHP website code, I would eventually return to the _best_ language of all: C++

...

Okay, maybe not _"best"_, but in my mind "C++ with a C style" (or imperative C++) was the least-worst programming language. I was never good at articulating _why_ I disliked other languages, but something bothered me about them, especially once things shifted to package managers.

When 2021 ended, I wasn't a C++ programmer.

Here's my story of how searching for a better way to blog broke who I was.


## Preamble: Static hosting, it's all in the deployment

GitHub has a product called [GitHub Pages](https://pages.github.com/) that publishes files from a repository as a static webpage. Obviously GitHub didn't invent static webpages, but I'd say GitHub's ubiquity lead to what came later. 

Static webpage hosting works as you'd expect it: upload some files and it shows up as-is. Static webpages are fast, efficient, and predictable.

How is it different from FTP'ing to an Apache server, like we did 20 years ago?

Generally speaking it's not that different, but workflows today are more refined. Files are stored in source control (GIT), with a full history of changes, commentary, and ticketing available (GitHub). While you can't run server-side scripts, you can customize the deployment process. Standardized tools can take a bunch of files and turn them into a website with consistent styles and structure. Test locally, push a change, and it's live a few minutes later.

Out of the box, GitHub Pages support the [Jekyll](https://jekyllrb.com/) static webpage generator. Jekyll is a Ruby tool that converts markdown files into HTML files. This is just one of many tools that do this, but if you use GitHub, it might be your first.

You can deploy to GitHub with tools other than Jekyll using "GitHub environments". They're a bit complicated to setup, so I leave that as an exercise to the reader. Alternatively you can create a 2nd repository, reference it as a GIT submodule, and store the output from a static webpage generator in that 2nd repository—I call this the "double GitHub repository" trick.

There's a problem though: You should only use GitHub Pages for non-commercial work.

Companies are free to use GitHub Pages, but if you drive a lot of traffic to something hosted on GitHub Pages, I'd worry it could be a problem.

Alternatively you can use a GitHub action to trigger an external service to pull the sources and rebuild. This is fine, but those other services are often not free, and "cost" is something I care a lot about.

A better alternative is CloudFlare Pages. CloudFlare Pages is a free static hosting service in the same vein as GitHub Pages, but you _CAN_ host commercial pages. They also greatly simply the workflow, no need to use GitHub environments and actions. You store your sources on GitHub, and CloudFlare Pages automatically deploys the changes as they are pushed to GitHub. You might also see this called [Jam Stacks](https://jamstack.org/what-is-jamstack/).

TL;DR: It's great. I love a good workflow.

But let's talk about the not-so-good workflows experienced on my journey.


## The Jekyll era (Ruby, Gem)

I ran a [WordPress](https://wordpress.com/) blog for over a decade (and a social network on it, `*cough*` Ludum Dare). By the end I was tired of, maybe hostile of WordPress.

Blogs are static.

They sometimes change, but never minute to minute, second to second. Why am I generating my blog for every request? This seems incredibly wasteful.

My other problem was I was styling my blogs with HTML. I don't hate HTML, but as a native document format it's clumsy. I was a fan of markdown now, having used it on GitHub for years. I rarely need the flexibility of raw HTML.

In early 2018 I migrated my WordPress blog to Jekyll.

The migration wasn't perfect. All of my image links and custom styles were now wrong, but my words were there. It was a blog again. Coming from WordPress, I missed editing my blogs from the browser. To get a bit of that back, I started using a plugin called [Jekyll Admin](https://jekyll.github.io/jekyll-admin/) that gives you a web interface for blogging.

For a while I was content with Jekyll, but the workflow was never as clean as WordPress. I took a break from blogging, but came back with no idea how to update my blog locally. There was a minor but essential step I could no longer remember, and I never found it mentioned in documentation or my own notes. The only way I could blog now would be to write, push the changes, and hope GitHub Pages wouldn't choke on them.

Okay, not the only way, but the alternative was spending hours re-learning Jekyll, Gem, and Ruby to diagnose the problem.

I was beginning to understand why I disliked modern programming languages.

Programming languages have environments, and modern language environments require package managers. JavaScript applications run on NodeJS, which needs NPM. Recent Python applications need PIP. Ruby is no different: you're not getting anywhere without Gem.

Package managers are chains of trust. When you require a package, you're not only depending on a package, you're trusting that package. Packages often depend on other packages, so to trust one package is to implicitly trust several other packages. Yes, I have trust issues, but those issues are well founded.

Trust is something I'm going to have to work on, but to my knowledge trust was never a tenant of Ruby. Trust isn't core to many languages. I don't mean trusting package managers as an attack vector (hello 20XX), but rather how seriously has the code in my chain of trust been vetted, and by whom?

When you depend on 3rd party code, that code becomes your responsibility.

The other problem is that every package manager is unique. They're similar, but never the same. The developers of Jekyll may say it's _"easy"_, but every _package-manager-powered-project_ comes with baggage: you need to learn the package manager (sometimes the language) to make the most of it.

In my experience, working with Jekyll/Ruby/GitHub pages was unreliable. GitHub Pages is fine, but I'd make a change to my files, push it, and it would silently error. I believe GitHub did eventually expose GitHub Pages errors in GitHub environments. I don't doubt some people have had a positive experience with this workflow, but I came to hate updating my Jekyll blog thanks to it.

The way forward was to commit to learning Jekyll, Gem, and Ruby, but I didn't see enough value in that.

My last post was in May of 2020.


## The Hugo era (Go)

I wasn't blogging anymore. My struggles with Jekyll and Ruby took the joy out of it. I had the itch though.

New plan.

Many static webpage generators support markdown. In theory, if I could find an alternative I liked, migrating shouldn't be _that_ much trouble.

I began searching for a Jekyll alternative.

The alternative I settled on was [Hugo](https://gohugo.io/). I'd learned the "double GitHub repository" trick, and I was willing to give it a try, though it did feel a bit hacky.

As hoped, migration went well. I like Hugo's file structure a lot better, and I was happier with my page styles. Improving on my Jekyll blog, I was able to create distinct sections for blog posts AND recipes. I could write, locally run a Hugo build, and push both my sources and the submodule. This new setup, in the moment, it was good.

But once I finished setting up, I didn't use it. I forgot how.

My mistake switching to static webpage generators was assuming I wouldn't have to learn them. I continued to approach blogging _"half assed"_, as I did with WordPress, and it bit me. With WordPress you don't have to think about files and structure. You write and that's it. Static website generators, at least the ones I wanted to use, required you to be fully aware of what goes in, and what comes out. It's not a mere application, it's a tool. Like a compiler for words. 

As before, I wasn't committed to learning Hugo and Go. I did try Go, but I didn't care for it. To make a post, I would have to re-setup and re-teach myself Hugo, then fiddle with GIT submodules. I wasn't looking forward to that. I also didn't have an _easy_ web interface to take the edge off. On top of all this, I was having a crisis of text editor. I wanted _desperately_ to move away from UltraEdit, a proprietary text editor I've used since the 90's. It was a series of "this just isn't working for me" events. 

To be fair there was a brand new pandemic going on, and many of us lacked motivation. To go further, I needed new inspiration.

My blog was stagnant.


## The Zola era (Rust, Cargo)

Fast forward to the summer of 2021... **HOLY SHIT!**

My business is thriving (by the way I started a business)! Contracts are signed, time is short, and I have webpages to make! 

**AHHHHH!**

None of this is "blogged" anywhere, as after a year I was still in limbo. My records are a few disjointed journals, video scripts, forum posts, and drafts that later became posts on [ldjam.com](https://ldjam.com). I'd either been unmotivated or too busy to solve my blogging problem, but as the ink dried I needed to revisit static website generators.

Weeks earlier I finished a proof-of-concept for [Jammer.tv](https://new.jammer.tv/) built with CloudFlare Pages and workers. If nothing else I knew CloudFlare Pages could host a static website. However my experience with Jekyll and Hugo left me ambivalent. I needed a static webpage generator, but which one?

I fired up the [CloudFlare Pages](https://developers.cloudflare.com/pages/) documentation and browsed. What _other_ tools do you support? Maybe one of them will catch my eye.

On the bottom of the list was [Zola](https://www.getzola.org/), a lightweight, single binary, static webpage generate written in Rust. I liked what I saw in Zola. Fast generator performance, no externals to worry about, and a _short_ feature list. Less features meant it could be mastered in less time.

Also Rust.

I'd actually been dabbling with Rust for a while, having made my way through [The Rust Book](https://doc.rust-lang.org/stable/book/) some months earlier. I was still very suspicious of package managers, though I couldn't yet articulate why (trust). Given my choices, Rust seemed the most likely modern programming language I'd personally use. Maybe this is what I need to get out of this software choice rut.

I don't have a better plan, so okay, let's try Zola.

...how do I even install it?

I'm a regular Ubuntu on _Windows Subsystem for Linux_ user (WSL), and on Zola's homepage I only saw a Snap package. You can't (couldn't?) use Snaps on WSL. I realized much later there were other options, but in the moment I'm stuck.

Shit.

I didn't have time to research another generator, so I grabbed my older Ubuntu laptop, installed the Snap, and soldiered on.

One long day later I'd pulled together the [Interactive Snacks](https://interactivesnacks.com) website. I started with one of the templates, and hacked it into something that met my needs. This was a simple website with pages, no blog. It was a hack-job and little gross, but it worked.

I dug deeper.

After combing over the docs for a few days, I had a pretty good idea of everything Zola could do. Again it's not the most featured static webpage generator, but if I stay in my lane, I knew exactly what it could do and how to do it. I tackled the first draft of [Ludum Dare's professional website](https://ludumdare.com). This website wasn't a blog, but it could be built with multiple blog-like feeds (folders).

* A news/press releases feed
* A schedule feed
* A sponsors feed
* A games feed (currently disabled)

Having taken time to ship a Zola project first (the Interactive Snacks website), then more time to learn Zola in depth, it made a huge difference when I sat down to plan this new website. I'm not going to say I execute it flawlessly, but I have so much more confidence now. I am confident Zola will be _enough_ to handle the needs of both, confident it'll work for more websites (this blog and the Academy), and I'm confident I can teach it to others.

I finally won.

Some time later, as I dabbled more with Rust, I figured out how to build Zola from sources (via Cargo).

Then my world came crashing down.


## Getting Rusty

Did you know: by default, Rust programs are built statically? In other words, when you compile a Rust program, you get a large binary without any _Rust specific_ or _Linux distro specific_ dependencies.

**WHAT!?**

Snap's and Flatpak's exist because of Linux distro fragmentation. Your Linux might ship with a different version of a library than mine. Sometimes that different version will work, but it's safer to assume not. Snap's and Flatpak's ship programs with exact copies of the libraries they require. There's more nuance then that, but that's them in a nutshell.

Snap's and Flatpak's are necessary because by default we assemble Linux programs dynamically. In other words, our programs make reference to functions in other files.

The Rust developers decided _nah_, lets make static linking the default. For standalone tools like Zola, that basically eliminates the need for Snap's and Flatpak's. Standalone Rust programs can be download and run simply, like an executable on Windows.

Heh. Remembering my panic, I laugh now knowing the _right_ way for me to install Zola was to visit the [releases page](https://github.com/getzola/zola/releases), download and unzip the binary, and move it to a `/bin/` folder in my path.

This was inconceivable. Linux is never that easy.

I think I've illustrated why it's vital you understand the environment, the package manager, and sometimes the language to use applications from an ecosystem.

Zola as a tool is great, but it's great because I was willing to learn it, Rust, and Cargo. That goes for any of these static website generators. You shouldn't use one unless you're willing to go _all in_.

So great, I have a static website generator I like now, but I have a new problem. 

## Feeling Rusty

I've been convinced for the past 20-some years that C++ was the _best_ or _"least-worst"_ programming language. I was proud that I knew the language so intimately, but I was critical of it. You'll find that many C++ professionals, especially those in games, **DON'T** go all-in with C++ features. C++11 is where most think C++ peaked (though the `auto` keyword is nice). The problem is the disconnect between the needs of developers _in the field_ and those working on the language. C++ today tries to be everything to everyone, and it struggles from the compromises and legacy it must maintain. It doesn't have gross namespace syntax like PHP, but it has a lot that'll make you ill.

C++ is an open standard with a committee of multiple vendors. For better or worse, it can't move fast and adapt like some projects.

Honestly I can't decide if committee or chaos is the better approach here, but after 20 years of feeling like C++ ignores game development, I'm ready for something new.

Rust and Go both came on to my radar around the same time. I didn't care back then, but since I started looking into it, Rust cemented itself as something different. The aggressiveness of the borrow checker and strictness of the language, while still having generous defaults, it intrigued me. It's not assumed to be a safer language because it has a garbage collector, it's a safer language because it will not compile until you've convinced it you're not doing something unsafe.

That's fascinating to me. It's as if [Valgrind](https://valgrind.org/) or [CPPCheck](http://cppcheck.net/) was built into the compiler, but it's so much more elegant than that.

It's trust. It's a programming language that prioritizes trust.

That's not to say you can't write malicious code in Rust, or get an algorithm wrong. But fundamentally Rust code will be more trustworthy than C++ code, or almost any code for that matter.

It's exciting, and that's just the language.

Other things the Rust project does is it has [working groups](https://www.rust-lang.org/governance) focused on different use cases, one of those being Game Development. I even recognize some of those names in the group.

Finally!

To be honest I haven't followed what the group is doing, but the fact that it exists at all is enough for me.

I wouldn't call myself a Rust programmer at this point, but I am absolutely a Rust enthusiast. I want to make time to do more with it. I want to use it in my day-to-day development.

I'm ready to move on from C++.

The final problem is that Rust is a package manager language. You can sort-of work around it, but no, it's not practical. If you Rust, you're using and trusting packages.

I'll never be 100% comfortable trusting 3rd party code. Sometimes I don't always trust my own code, but I take responsibility for it. To Rust's credit, coding in Rust is so much more pedantic than any language I've used. It forces you to write safer code, or highlight the unsafe, which should make reviewing 3rd party code easier.

You should always review 3rd party code before you use it, but if nothing else you can trust it had to pass the rigorous scrutiny of `rustc`.

It's worth a try.

That's where I'm at. I was a C++ programmer, but then I dared search for a new way to blog.