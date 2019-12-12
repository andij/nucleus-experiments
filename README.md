# nucleus-prototype

A Nucleus prototype environment using eleventy and less compiler.

Learnings from:

https://www.11ty.io/docs/getting-started/

https://www.npmjs.com/package/less-watch-compiler

## Installation

Clone or download the GitHub repository: https://github.com/andij/nucleus-prototype

Open the /nucleus-prototype folder using a favourite terminal. Mine is [iTerm2](https://iterm2.com/).

Assuming Node.js https://nodejs.org/ is installed.

### Install the npm packages

```bash
🔹  npm i
```

## Starting the environment

Continue running a few more commands in terminal.

### Run the less-watch-compiler to compile the styles

This will produce the `main.css` in the `/dist` folder.

```bash
🔹  npx less-watch-compiler
```

#### Optional

Anytime the styles for the nucleus-prototype are changed, run this command.

If lots of styling is being worked on, change `runOnce` flag in `less-watch-compiler.config.json`

* From

```json
"runOnce": true
```

* To

```json
"runOnce": false
```

This will continue to watch the `_styles` folder for any changes and recompile `main.css`.

### Launch Eleventy
```bash
🔹  npx @11ty/eleventy --serve
```
Open your browser at http://localhost:8080 and relish in the vanilla prototype environment you have started.

Using `_layouts/basic.njk` will show the page as raw HTML, with no styles and no javascript.

## Connecting Nucleus

This step is where we pick up the CDN of the Nucleus library and include it into our template.

The `_layouts/live.njk` includes the Nucleus javascript library at `https://nucleus.bgdigital.xyz/nucleus.min.js`.

Change the layout file in the frontmatter of index.md

* From

```json
layout: basic.njk
```
* To

```json
layout: live.njk
```
Refreshing the browser will show the Nucleus font. Indicating that Nucleus is available.

## Developing Web Components using nucleus-prototype

Introducing local development of Nucleus alongside this prototype environment is achieved by creating a symbolic link and changing the location of scripts.

Assuming Nucleus https://github.com/ConnectedHomes/nucleus is installed and running locally with PaDL https://github.com/britishgas-engineering/padl.

Create a symlink in the `/dist` folder with the name of `nucleus` pointing to `~/Projects/nucleus/dist` allows these two projects to adjoin.

Navigate to `~/Projects/nucleus-prototype/dist`

```bash
🔹 ln -s ../../nucleus nucleus
```

This will generate a symlink `nucleus -> ../../nucleus`

Switch the layout to `_layouts/local.njk` and refresh the browser.

The addition of the symlink, and referencing the local Nucleus scripts using this symlink it's now possible to include the local development of Web Component statically.

### Displaying a Nucleus page

In the `/src` folder we create a folder. Any name will do, let's use `/pages`.

create a Markdown file `my-page.md` and include a `title` and a `layout` in the frontmatter.

```json
---
title: Page title
layout: live.njk
---
```

We can then include any Nucleus components. Let's start with the Landmark:

```html
<ns-landmark type="hillside">
  <h1 slot="heading">
    <span class="h5">Hub umami locavore.</span>
    <span class="h1 enlighten">Typewriter pin <b>chambray mixtape</b></span>
  </h1>
  <div slot="paragraph">
    <p>Cred sartorial shaman pitchfork mumblecore braid cronut shaman gastropub taiyaki godard roof party. <a href="#caveat" aria-label="Additional information 1">1</a></p>
  </div>
</ns-landmark>
```

Then add some more stuff.

```html
<ns-panel>
  <div class="splish">
    <h2>Bushwick kitsch truffaut bespoke stumptown</h2>
    <p class="p-feature">Coloring book palo santo drinking vinegar twee heirloom iceland la croix listicle.</p>
  </div>
  <div class="splish triple" role="list">
    <ns-card role="listitem" type="section" decoration="gas">
      <h3 slot="heading">Sriracha hashtag fixie neutra</h3>
      <div slot="paragraph">
        <p>Twee cronut iceland la croix party listicle shaman.</p>
      </div>
      <a slot="cta" href="#!">
        <ns-cta type="direct">Distillery kinfolk</ns-cta>
      </a>
    </ns-card>
    <ns-card role="listitem" type="section" decoration="home">
      <h3 slot="heading">Organic</h3>
      <div slot="paragraph">
        <p>Bushwick tumeric before they sold out.</p>
      </div>
      <a slot="cta" href="#!">
        <ns-cta type="direct">Migas franzen drink</ns-cta>
      </a>
    </ns-card>
    <ns-card role="listitem" type="section" decoration="boiler">
      <h3 slot="heading">Bespoke stumptown</h3>
      <div slot="paragraph">
        <p>Knausgaard flannel organic tote bag prism pug.</p>
      </div>
      <a slot="cta" href="#!">
        <ns-cta type="direct">Art party</ns-cta>
      </a>
    </ns-card>
  </div>
</ns-panel>
```

Repeat the process, create other pages etc.

🤩
