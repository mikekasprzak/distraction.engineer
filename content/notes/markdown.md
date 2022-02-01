+++
title = "Markdown"
+++

Prefer CommonMark to Markdown: <https://spec.commonmark.org/0.30/>

Zola uses the pulldown-cmark procesor: <https://github.com/raphlinus/pulldown-cmark>


## Interesting extensions
* GFM - A few extensions from GitHub (tables and better autolinks) - <https://github.github.com/gfm/>
* MyST - Book and Math addons - <https://myst-parser.readthedocs.io/en/latest/index.html>


## Things I missed
* The `<https://autolink>` standard
* Ending a line with an escape `\` inserts a `<br />`
* You can escape symbols with a `\` to disable processing. Example: `\* not a bullet`
* HTML entities are suported.
  * `&copy;` for &copy; and `&trade;` for &trade;
  * `&star;`, `&starf;`, `&heart;`: &star; &starf; &heart;
  * `&dagger;` for &dagger;, `&Dagger;` for &Dagger;, `&loz;` for &loz;
    * More here <https://sites.psu.edu/symbolcodes/codehtml/#punc>
  * Em and En dashes are `&mdash;` and `&ndash;` respectfully. 
    * Use hyphens for words. pre-game, jack-o-lantern.
    * Use En dash for ranges (April 1st&ndash;4th), intersections (Oxford&ndash;Wonderland), and compound two word adjectives (post&ndash;Cold War)
    * Use Em dash a lot of places.
      * As a comma, or alternative to a parethetical `()`. Usually when something is slightly off topic or detatched.
      * To suggest someone was cut-off in speech. "The Award for&mdash;" "Im'a let you finish, but..." 
    * <https://www.merriam-webster.com/words-at-play/em-dash-en-dash-how-to-use>
