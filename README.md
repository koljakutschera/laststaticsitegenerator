# LastStaticSiteGenerator

After learning how to use task runners like grunt, gulp, webpack and many static site generators i was in need for one workflow that makes setting up any tools unnecessary. So my first mission was to find tools that require minimal or no configuration for all parts of my frontend-workflow. The first tool must be a static-site-generator for creating multi-page websites. The second tool is an asset-bundler that just handles any file i put into my websites like .css, .js, .ts, .vue and all my node_modules from npm. The last third tool should pick up all generated files and transform them into a fancy progressive-web-app (pwa) with offline-support and everything just done with support for multiple languages.

After testing many fancy tools i found the following three to be the best for my use-case:

- Static-site-generator: Eleventy
- Asset-bundler: Parcel
- PWA-generator: Workbox

I tried to turn my new toolset in a row via simple npm-scripts. First generate static websites with Eleventy. Than Parcel automatically extracts, transforms and even downloads all needed assets from the static websites to create some bundles. At last Workbox comes in to the game and does its magic to generate a minfied PWA with all batteries included from these bundles.

I finally got things working and i did not need any other setup in the last few projects. No matter if its react, vue or some good old jquery. So if this can make me happy i think it must be shared...

# Setup

Get https://nodejs.org and use a Unix/Linux system because i have not tested this on win.

## Installation

1. Clone this repository:

```
git clone https://github.com/koljakutschera/laststaticsitegenerator.git
```

2. Open a terminal and run:

```
npm install
```

## Development

To generate, bundle and serve the site on: http://localhost:1234/index.html open a terminal and run:

```
npm run dev
```

or with https enabled:

```
npm run dev:https
```

## I18n

Language files are located at: locales/ using ISO-Codes as filename.

To set the language in development use the environment variable: ELEVENTY_LOCALE in front of npm commands. If you add new languages add them in package.json to all ELEVENTY_LOCALES environment variables.

Example: Set available locales (package.json)

```
ELEVENTY_LOCALES=(en de)
```

Example: Set current dev locale

```
ELEVENTY_LOCALE=de npm run dev
```

## Production

To generate a minified production-bundle open a terminal and run:

```
npm run production
```

or the same with multiple languages:

```
production:i18n
```

To generate the minified production-bundle as PWA open a terminal and run:

```
npm run production:pwa
```

or the same with multiple languages:

```
production:pwa:i18n
```

To just serve the production-bundle from "dist"-directory on localhost open a terminal and run:

```
npm run serve
```

## Getting started

If you are ready for coding check the "site"-directory which contains some example files. Now its time to read the very short/good eleventy and parcel documentations. You can adjust everything workflow related in package.json's scripts section.

## Getting started with PWA

If you want to generate a PWA in production you can adjust the following files to your needs:

- site/\_data/metadata.json
- site/manifest.njk
- site/\_assets/manifest-192.png
- site/\_assets/manifest-512.png

Advanced users can use: workbox-config.js to configure how the PWA builds. For more infos see: https://developers.google.com/web/tools/workbox/

## Supported technologies

HTML, Markdown, Liquid, Nunjucks, Handlebars, Mustache, EJS, Haml, Pug, and JavaScript Template Literals, Javascript, ReasonML, CSS, SCSS, LESS, Stylus, HTML, TypeScript, CoffeeScript, Vue, JSON, GraphQL, Rust, WebAssembly, YAML, TOML, OpenGL, Pug, WebManifest, ...

## Some fancy features

- no plugins
- code splitting
- filesystem cache
- minimal configuration
- multicore compilation
- hot module replacement
- out of the box support for everything\*
- use legancy scripts and node_modules side by side

## More documentation

Eleventy: https://github.com/11ty/eleventy

Parcel: https://parceljs.org/

Workbox: https://developers.google.com/web/tools/workbox/

## FAQ

### Why no subdirectories/folder-urls

By default Eleventy creates subdirectories/folder-urls when generating pages but because Parcel cant work with them i used a little hack or better a feature (permalinks) of Eleventy itself to rewrite all pages to the root directory. Maybe this will be fixed by Parcel in feature.

### What are the .cache, .cache_eleventy, .cache_parcel directories for?

Temporary files. Just ignore them.

## Troubleshooting

### Build error in development

When the development task is started first parcel needs to wait for eleventy to build pages. This usually takes < 1 second but if you have thousends of pages it can take a little more time. In this case you can adjust the time parcel has to wait for eleventy in package.json. Look for "sleep 3" and adjust it to the time eleventy shows you in the console. For example eleventy shows: Processed 7000 files in 5.28 seconds so you should put something like sleep 6 in package.json.

### I see nothing on: http://localhost:1234/

Append your pages filename to the URL for example: index.html = http://localhost:1234/index.html

### PWA Manifest not served

Make sure your server uses: application/manifest+json MIME-Type for .webmanifest file.

### Serviceworker not loaded

The serviceworker will not load on local domains (localhost/127.0.0.1).

### Page not generated

This is because parcel does not(now) work with sub-directories(or folders-URLs) created by eleventy. To fix this we rewrite permalinks in eleventy from: /filename/index.html to /filename.html via eleventy's global-data-file feature(see site/\_data/permalink.js). You can overwrite the default permalinks via yamlfront in a template but you have to make sure that its not rendered to a sub-directory. The following will not work for example:

```
---
permalink: subdirectory/filename.html
---
```

But this will work:

```
---
permalink: filename.html
---
```
