+++
title = "Unfailing to blog, by fail-blogging"
+++
Two years ago, kicking off 2022 with a bang, I attempted to revive and rebrand my blog. At the time I rambled something about my love of writing, but in hindsight writing is the best tool I've found to focus my ADHD scatterbrain. Committing words to page, order to ideas, endlessly feeding the dopamine craving machine between my ears.

The new super-blog would combine several ideas. I had a cool new branding and identity to explore (`the Distraction.Engineer`). I'd separate my "note-blogs" from the main feed, posts written as I did tasks to help me reproduce them. I'd also add a section for recipes, as cooking is something I enjoy and overthink. I'd rebuild and repopulate my portfolio, something I started on [me dot com](https://mikekasprzak.com) but never finished. I could even add an active projects section, per-project blogs, a recent videos section, and much more. Finally, I'd build it using [Zola](https://www.getzola.org), an exciting new piece of software I discovered.

Hoo boy, there were grand plans! And yes, this was a rage project! I would quit wasting my words on Elon's X-Twitter, quit getting sucked into pointless dramas, and focus on collecting words here, on a website I controlled! Raaaage!

I ended up rushing the design, which on one hand was good 'cause it meant I could start writing. My overly critical self got the better of me. I accumulated many drafts that never satisfied me enough to publish, not to mention the posts and site design I did have didn't inspire me enough to want to share. A few short months later, another New Years resolution was dead.

What went wrong?


## The Jam-stack 

No, Jam-stack has nothing to game jamming. It's a nickname given to a movement of static website generation workflows. Instead of authoring pages in HTML or relying on WordPress, you author them in an intermediary format like Markdown text files, then generate HTML pages from them using a tool like Jekyll, Hugo, and in my case [Zola](https://www.getzola.org/).


### Publishing troubles and Zola

As mentioned, this blog is built using a piece of software called [Zola](https://www.getzola.org/). The blog itself is stored in a GitHub repository, and I've configured CloudFlare's "Pages" service to automatically publish the changes it anytime I push them. My workflow is exactly that: push a change to the Git repository, CloudFlare detects the change and spins up a Linux VM. The VM runs an install script, pulls the Zola binary, pulls my repository, runs a `zola build`, and if there are no errors it publishes the resulting HTML files as static files on CloudFlare's free "Pages" hosting service. Nice!

It is, when it works.

After running and continuing to improve my blog, I hit a snag.

Zola is a standalone binary built in Rust. As Zola recieved updates, the minimum system library versions required to run the Zola increased. This was no problem for my local copy, but many users discovered that most Jam-Stack providers didn't have the updated libraries. So unfortunately, even though key issues were fixed in the latest Zola versions, I got stuck running an older version thanks to my workflow.

The dependency issue was eventually addressed in a later version, but as an open-source freeloader _(hi there)_, I was at the mercy of both CloudFlare and the core developer.

This was my mistake, but I don't regret it. I wanted to use and support this shiny new thing, and on the bleeding edge you sometimes bleed. Running into problems operating a personal blog is fine, but at least I wasn't using Zola on an important website...


### Big league Zola

Almost right away, I put Zola into production on [Ludum Dare](https://ludumdare.com). Oops!

Things started out peachy, but when the inability to update the Zola version cropped up, I got bit.

For what I call the Ludum Dare information website, I used Zola as a CMS. The plan was for this website to host the rules, the FAQ, and any other important documents separate from the event website. The [event website](https://ldjam.com) can be a chaotic and overwhelming place, and I wanted a "more chill" website to introduce the event.

For the most part this went well. Zola is extremely flexible in how you can structure pages, and I was able to build-out the information website exactly as intended. But as mentioned, I ran into the version updating problem. Because the content on <https://ludumdare.com> is important for running events, I did whatever to just make it work. While I always had the option of switching to another Jam-stack, the issues were more troublesome to me as the author than my users.

I decided to "just live with it", for now, knowing that _some day_ it would get fixed.


## Authoring content

[Zola](https://www.getzola.org/) isn't the only static site generator I've used. A decade ago I ugly-migrated my WordPress blog to [Jekyll](https://jekyllrb.com/), as Jekyll support was built-in to GitHub. 

Running Jekyll locally always gave me trouble. It was never clear to me if it was a quirk of Jekyll or something I misunderstood about the Ruby runtime. In short, there were numerous times my changes wouldn't change anything, or it would spit out blank pages with no errors. Teaching myself Ruby so I could understand why seemed wasteful, but more importantly I found that I wasn't happy with the authoring experience of editing text files. 

### Giving up UltraEdit

At the time I was still using UltraEdit, a "hardcore yet basic" text editor for Windows and eventually Linux. I used UltraEdit as my primary text editor and IDE for 20 years. It did basic syntax highlighting, and I knew the my hotkeys. The company eventually released a proper IDE product, but I wasn't interested. I was more than happy with just the text editor.

UltraEdit did you no favours. At the time it had zero support for mixed-content documents, meaning displaying a Markdown file with TOML front-matter or JavaScript with JSX wasn't possible.

I worked for decades without intellisense. Part of me was probably trying to prove a point, but not having it did help me develop some good habits. UltraEdit does support something called [Ctags](https://en.wikipedia.org/wiki/Ctags), but even when they were enabled, I don't recall the experience being any good. It was no Visual Studio.

For me, the final nail in the coffin was Rust. Having to alt+tab then compile to check every pedantic change seemed an impractical way to work. I could get by writing JavaScript+JSX code in UltraEdit well enough, but whatever point I set out to prove I've long forgotten. I wasn't looking forward to re-learning my shortcut keys though.


### The pain of updating front-matter

While techincally 



### Managing images

With WordPress, you click a button to upload an image. After uploading, perform basic manipulations (cropping/sizing), making authoring posts with images a breeze.

In a Jam-stack

