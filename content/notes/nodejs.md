+++
title = "JavaScript Runtimes (NodeJS, Bun, Deno)"
description = "Making Javascript run places"
+++
Some ramblings. Don't treat this as gospel! It's just a notebook.

# NodeJS version management sucks
When you Google how to install it, you get bad results (or at least I always do). Following the official recommendations sucks.

Some better options.

## NVM - Node Version Manager
* <https://github.com/nvm-sh/nvm>
* <https://github.com/nvm-sh/nvm#installing-and-updating>

```bash
# To install NVM, grab the latest instructions from above
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash

# To install a specific version
nvm install v18
```

## N - Node Version Management (package)
If you have node already installed, you can use this package to install other versions.

* <https://github.com/tj/n>

```bash
# Install the N package
npm install -g n

# Install node version
npx n latest # (or current)
npx n lts
npx n 18
```

# Bun is awesome
* <https://bun.sh/>

It's essentially a faster drop-in replacement for Node. Compatible with TypeScript. Written in Go.

```bash
# Install Bun
curl -fsSL https://bun.sh/install | bash

# Update bun
bun upgrade

# Using bun
npm install
bun install

npm run script
bun run script

npx packagename
bun x packagename

node ./app.js
bun ./app.js
```

Bun also works as a bundler, but their priorities are replacing Node, so webdev features like targetting older browsers aren't supported.

Thus far it's worked perfectly for me, with the sole exception a networking bug with `ESBuild`.


# Deno, the last Dinosaur
* <https://deno.com/>

Built as a more secure alternative to Node. Compatible with TypeScript.

I haven't done much with this yet, but I'm a fan.

## Fresh - A native Preact powered web framework
* <https://fresh.deno.dev/>

In an exciting turn of events, the Deno folks hired Marvin Hagemeister, the maintaner of Preact to run the project (and work on Preact). Much excite.


# ESBuild is a great tool
* <https://esbuild.github.io/>

It's not a runtime, but ESBuild is an incredibly fast bundler, minifier, and transpiler all in one! It supports plugins and live coding (though the file scanner is a bit slow).

Bun is technically faster (insignificantly so), but Bun doesn't support the same bredth of processing that ESBuild does.

That said you can run ESBuild with Bun, which dramatically speeds up ESBuild times. Win win.

## Import CSS files in JS
When you import CSS files into your JS app, the bundler detects this, and bundles up an output `.css` file for you of all the classes.

```js
import "./styles.css"; 

// do JS here
```

## CSS Modules in ESBuild
ESBuild supports the CSS Modules. This adds a few new pseudo classes and keywords to the CSS dialect, and improves the import syntax.

```css
/* style.css */
.my_class {
    font-weight: bold;
}
```

```js
import {my_class} from "./style.css";

const MyButton = (props) => <button class={`${my_class} ${props.class ?? ''}`} />;

```

Behind the scenes, `my_class` holds a string that the minifier can change, keeping both the output JS and CSS files in sync. By default it'll use a simple name like `style_my_class`, but once minified it'll be something like `j`.

