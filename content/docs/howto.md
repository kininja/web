+++
date = '2026-03-17T21:35:11-07:00'
draft = false
title = 'How to set up this site'
+++

# Start of a new page

Here's my notes so far for setting this all up...

Hugo, new theme install, Lotus docs looks insane: https://github.com/colinwilson/lotusdocs

```
hugo mod init github.com/kininja/web
```

```
kinji@skycatcher web % which hugo
/opt/homebrew/bin/hugo
kinji@skycatcher web % hugo mod init github.com/kininja/web
ERROR failed to init modules: binary with name "go" not found in PATH
kinji@skycatcher web %
kinji@skycatcher web % hugo mod init github.com/kininja/web
go: creating new go.mod: module github.com/kininja/web
hugo: to add module requirements and sums:
        hugo mod tidy
kinji@skycatcher web % hugo mod tidy
kinji@skycatcher web %
```

I brew installed go.

```
hugo new docs/example-page.md
```

```
kinji@skycatcher web % hugo new docs/example-page.md
kinji@skycatcher web % hugo new docs/example-page.md
ERROR failed to load config: "/Users/kinji/git-kininja/web/hugo.toml:3:24": unmarshal failed: toml: literal strings cannot have new lines
kinji@skycatcher web % vi hugo.toml
kinji@skycatcher web % vi hugo.toml
kinji@skycatcher web % hugo new docs/example-page.md
go: no module dependencies to download
go: downloading github.com/colinwilson/lotusdocs v0.2.0
hugo: downloading modules …
go: added github.com/colinwilson/lotusdocs v0.2.0
go: downloading github.com/gohugoio/hugo-mod-bootstrap-scss/v5 v5.20300.20800
go: added github.com/gohugoio/hugo-mod-bootstrap-scss/v5 v5.20300.20800
hugo: collected modules in 18555 msContent "/Users/kinji/git-kininja/web/content/docs/example-page.md" created
```

Whoops, didn't single quote close on the name. Fixed, we're off and running. 

Ok, looks good in `hugo server -D`

Let's add a Github action in `.github/workflows/hugo.yaml`:


```
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true
      - name: Build
        run: hugo --minify
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**Github** -> **Settings** -> **Pages** -> **Github actions** (instead of deploy from branch). 

Add DNS domain below.

Add domain in **Profile** -> **Settings** -> **Code, planning and automation** -> **Pages**, add a domain.

We could optionally add a domain to an org or not here. 

Steps will then generate needed dns changes, `TXT` and `CNAME`s.

And we're up and running! Next steps... learn how to use the theme. 
