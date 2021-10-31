# Eleventy Starter (Advanced)

First time setup:

- [Click this link and make a new
repo.](https://github.com/dustinwhisman/eleventy-starter-advanced/generate) This will
open a form for you to create a new repo with all the files from this one.

- Out of the box, you should have:
  - [x] Eleventy configured to build your templates into pages, including a
    catch-all 404 page
  - [x] Sass support with minimal/brutalist styles ready to go, including dark
    mode
  - [x] JS bundling, minification, and code splitting
  - [x] Minimal PWA requirements already met
  - [x] A service worker with precaching and a basic
    cache-falling-back-to-network strategy in place
  - [x] Documentation explaining what to change, what to delete, and how things
    work

Things to update:
- [`package.json`](./package.json)
  - `name`: change to the name of your site
  - `author`: change to your name
  - `license`: change if you want to use a different license
- `README.md` (that's this file!): replace with info relevant to your project
- `src/assets/fonts`: add any fonts you want to use, if self-hosting
- [`src/assets/scss/settings/_fonts.scss`](./src/assets/scss/settings/_fonts.scss): add any `@font-face` declarations you need
- [`src/assets/scss/settings/_theme.scss`](./src/assets/scss/settings/_theme.scss): change fonts, colors, etc. to match the style you want
- Any other styles that you don't like or need to change
- [`src/pages/index.njk`](./src/pages/index.njk): replace with your home page content
- [`src/pages/404.md`](./src/pages/404.md): replace with a 404 page that you want
- Icons, manifest, and robots.txt
  - favicons, for both `svg` and `png` formats, for both light and dark modes
  - [`src/root/manifest.json`](./src/root/manifest.json)
    - `name`: the full name of your site
    - `short_name`: abbreviated name for your site
    - `description`: describe your site
    - `icons`: point these at the new icons you replace, if you changed the names
    - `theme_color`: probably your primary brand color
    - `background-color`: background color for the splash screen
  - [`robots.txt`](./src/root/robots.txt): update if you want to block certain robots/pages
- [`src/templates/components/header.njk`](./src/templates/components/header.njk): replace with markup for your header
- [`src/templates/layouts/blog.njk`](./src/templates/layouts/blog.njk): change any part of the layout you don't need or want changed, or delete if not needed
- [`src/templates/layouts/default.njk`](./src/templates/layouts/default.njk): change any part of the layout you don't need or want changed
- [`.lighthouserc.js`](./.lighthouserc.js): change the list of URLs to match pages that you want audited by Lighthouse, or delete them and uncomment the `maxAutodiscoverUrls` line to audit all pages

Things to delete:
- `src/assets/js/example.js`: the example JS file that isn't actually useful, as
  well as the `add.js` and `subtract.js` utility JS files
- `src/pages/blog`: example blog entries (the index page might still be useful,
  though)
- `src/pages/docs`: documentation geared toward developers
- Any other pages or templates that won't be needed

## Getting Started

1. Before running the project, set up node/npm with the latest versions (check
   the "engines" section in `package.json` for details).
1. Run `npm install`
1. Run `npm start`. This will
  - Build project files
  - Start the development server (`http://localhost:8888`)
  - Run watch tasks on Sass/JS files as well as your templates
  - Listen for requests to serverless functions

### File Structure

Most of the work that you do will be done in the `src` directory. If you find
yourself changing configs outside of `src`, consider forking this repo, making
the changes there, and then using that as the base for future projects.

#### Assets

The `assets` folder is where your fonts, images, CSS, and JS will live. By
default, the `fonts` and `images` folders are empty, so all you need to do is
add fonts/images as needed. They'll be available publicly from `/assets/fonts`
and `/assets/images`.

When referencing fonts from CSS or SCSS, the relative path of `../fonts` should
get you what you need.

There are some JS files and tests for those files already in the `js` folder.
You can delete those once you understand how bundling works by default for this
project (see below for more details).

The `scss` folder already contains some basic, brutalist styles. You can rip
those out and replace them with your own if you like, use them as-is, or extend
them, but they should be decent enough to get you started with simple, blog-like
pages without much effort. When you run the project, check out
`http://localhost:8888/docs` to see more about the default styles.

#### Pages

The `pages` folder is where you will put the actual pages that will get URLs.
This project mostly uses Nunjucks (`.njk`) and Markdown (`.md`) files for pages,
although you could probably use other templating languages that Eleventy
supports (do so at your own risk – this wasn't built with all of them in mind).

Routes are generated by convention, so the structure of your site will closely
match the file structure of the `pages` folder (unless you use lots of
permalinks).

The `blog` folder has a few example posts written in Markdown and an index page
that has a paginated list of links to each post. Take a look at how they work
with regards to collections and dates, then feel free to delete them.

The `docs` folder is aimed specifically at developers. Check them out, then
delete them when you no longer need them (or you don't want them public on your
site).

The `_data` folder contains data that can be accessed from your pages or
templates. It is currently used to generate random version numbers for the
service worker, but it can be used for tons of other things, including fetching
data from a CMS.

Worth noting is the `service-worker.njk` file here. It's a bit unusual, but you
can use a `permalink` to treat a `.njk` file as a JS file. The `permalink` is
`/service-worker.js`, which lets us use Eleventy variables in a file that will
be output as JS.

#### Root

The `root` folder is where the things that will live at the website's root go.
This includes icons, the site manifest, and robots.txt. Nearly all of these
files are worth updating or replacing, depending on the needs of your site.

The favicon files, as well as the `mask_icon.png` and `splash_icon.png` files,
should be replaced by appropriate versions for your site. If the file names
change, or you decide not to use dark mode icons, make sure to update the places
that reference them.

By default, this project includes an empty `robots.txt` file, which will allow
any and all robots to crawl all pages of your site. If you want to change that,
update `src/root/robots.txt` with your rules (more info
[here](https://developers.google.com/search/docs/advanced/robots/intro)).

#### Templates

The `templates` folder contains partials, components, and layout templates that
are used by the pages in the `pages` folder. Except for `layouts`, these are
used by using `include` statements in Nunjucks.

```njk
{% include "components/header.njk" %}
```

Layouts can be referenced in Front Matter or they can be used via `extend`
statements.

It generally makes sense to define the layout in Front Matter.

```md
---
layout: layouts/default.njk
---
```

However, if you want to override blocks, it's better to extend the layout. See
the docs for [templates](http://localhost:8888/docs/templates) for more info.

```njk
{% extend "layouts/default.njk" %}
```

#### Functions

The `functions` folder contains all the serverless functions that can be used by
the site. The name of the file determines the path, so for example, `hello.js`
can be accessed by fetching `/api/hello`. Behind the scenes, Netlify, or Netlify
CLI will redirect the request and respond accordingly based on the function.

## Project Details

Here are some details about how the project is set up and how the different
pieces work together. If you're already familiar, feel free to delete this
section from the `README`.

### Eleventy

This uses [Eleventy](https://11ty.dev) as its static site generator. It supports
many different kinds of templating languages, so check out the
[documentation](https://11ty.dev) to see what you can do with it.

### Sass

This project uses [Sass](https://sass-lang.com/) to compile into CSS.
Boilerplate styles are in the `src/assets/scss` folder and are output to the
`dist/assets/css` folder.

Sass is organized into folders following an ITCSS convention. The folders are as
follows:
- `settings`: global variables, colors, and fonts should go here
- `tools`: mixins, functions, keyframes, etc. should go here
- `generic`: normalize/reset-type styles, or anything not specific to this site
  should go here
- `elements`: styling for base elements (such as `h1` or `a`) should go here
- `objects`: broad, undecorated design patterns should go here – this is the
  first layer that uses class selectors
- `components`: specific UI components should go here – this is the bread and
  butter
- `vendors`: third party styles should go here so they live side-by-side with
  your own styles, but can still be overridden by utilities
- `utilities`: utilities and helper classes should go here – this is where you
  might use `!important` extensively

### JavaScript

JavaScript files are processed by esbuild, which bundles/minifies JS and handles
code splitting. The output is intended for browsers that support ES modules, so
if you need support for legacy browsers, you'll need to add support yourself. If
you do generate legacy builds, be sure to use the `module/nomodule` pattern to
prevent browsers from loading/running the wrong scripts.

By convention, any files in `src/assets/js` will be treated as entry points, and
the processed output will be written to `dist/assets/js`.

```html
<script src="/dist/assets/js/scripts.js" type="module"></script>

<!-- use this for legacy bundles or fallbacks for older browsers -->
<script src="/dist/assets/js/legacy/scripts.js" nomodule></script>
```

### Linting and Testing

This project uses [stylelint](https://stylelint.io/) to help improve the quality
of SCSS and CSS code. Run `npm run lint:css` to check for any linting errors. To
change the rules, edit `.stylelintrc.json`.

This project uses [eslint](https://eslint.org/) to lint JS. Run `npm run
lint:js` to find any linting errors. To change the rules, edit `.eslintrc.json`.

This project uses [Jest](https://jestjs.io/) to test JS. You can run `npm run
test:js` to check that your tests pass.

By default, when creating a pull request, GitHub actions will run the
`lint:css`, `lint:js`, and `test:js` scripts to prevent poor quality code or
failing tests from ending up in `main`. To change that, you can edit or delete
`.github/workflows/code-quality.yml`.

### PWA Requirements

Manifest files, default icons, and the correct meta tags should get you most of
the way to having a working PWA. You will need to update theme colors and icons,
but the infrastructure should already be in place.

Files to update or replace (as desired):
- `src/root/favicon.png`
- `src/root/favicon-dark-mode.png`
- `src/root/favicon.svg`
- `src/root/favicon-dark-mode.svg`
- `src/root/manifest.json`
- `src/root/maskable_icon.png`
- `src/root/splash_icon.png`
- The `<meta name="theme-color" content="#202020">` tag in any layout templates

### Service Worker

The default service worker will precache the home page and the main CSS file,
and it will ignore any URLs from `localhost`. All requests will follow a
cache-falling-back-to-network strategy. Every time the site is built by
Eleventy, a new version number will be given to the service worker, which should
keep you from being bit by cached resources on deploys.

You can specify more routes to precache or exclude, as well as any hostnames to
exclude, such as a CDN, for example. To add more caching strategies for
different types of requests, check out the [Service Worker
Cookbook](https://serviceworke.rs/) by Mozilla or [The Offline
Cookbook](https://jakearchibald.com/2014/offline-cookbook/) by Jake Archibald.

### Lighthouse Audits

This project is set up to run Lighthouse audits on selected pages for every pull
request. They are following `lighthouse:recommended` settings, but you can
change that in `.lighthouserc.js` to be more or less strict.

Failed audits will block pull requests from being merged. Out of the box, you
should see perfect 100/100 scores, so keep an eye on that as you develop, and
make sure to investigate any errors or recommendations that are surfaced by
Lighthouse.

### Firebase Authentication

This project uses Firebase for authentication, and it protects serverless
endpoints through middleware. The code part should be taken care of already, but
you will need to set up some things in Firebase to get up and running.

#### The Firebase Console

If you don't have a [Firebase](https://console.firebase.google.com) account
already, you will need to sign up for one. There's a generous free tier, so
unless you are going to have loads of users, it should be sufficient.

##### Creating a Project

From the console, add a project, and give it a name. Your call if you want to
enable Google Analytics, but none of the code in this project is written to
support it. Once Firebase is finished setting the project up, you should see
some "Get started" message and options for adding Firebase to your app. You want
the Web option.

From there, you can add an app nickname, and choose whether to use Firebase
Hosting (probably not, since this assumes Netlify as your hosting provider).
Once you've registered the app, you should see a code sample with API keys and
other project information. You'll want to copy those values and add them to your
`.env` file (see [`.env.example`](./.env.example)) for more information.

##### Configuring Authentication

Now you should be ready to set up authentication. In the Authentication
settings, add Email/Password as your provider. Enable Email/Password and the
"Email link (passwordless sign-in)" options, then save.

From here, you can add whatever domains should be authorized. `localhost` should
be there by default, but you'll also want to add any staging, preview, or
production domains that your site will exist on. If you don't have a domain set
up yet, you can come back to this later.

You can also edit the default email templates if you wish. One thing you'll want
to do is change the public-facing name of your project so users don't see a
garbled string with a bunch of numbers as the site name when they try to log in.
You should be able to find that under general Project Settings as "Public-facing
name".

##### Finishing Up

There are other settings you can change here that aren't strictly necessary, but
might be helpful, such as "Default GCP resource location" and "Support email".

From Project Settings, you'll need to go to Service Accounts and generate a new
private key. This will create a JSON file with a bunch of sensitive information.
Once you download the file, you'll want to remove any newlines and copy the
string into your `.env` file for the FIREBASE_ADMIN_CREDENTIALS variable.

With all of that done, you should be able to log in to your site, hit protected
endpoints, and manage your account without issue.
