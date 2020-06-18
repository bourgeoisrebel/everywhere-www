---
title: "First steps in to the world of building a small company website"
date: 2020-06-16T21:39:53+01:00
draft: false
toc: true
---





___

## Building the Hugo MVP on Azure Static Web Apps

>the hardest part of the journey is always the first step

Creating a website is one of those things that was on the to-do list for a long time, but never quite made it to the top. Work comes via networking, word of mouth and contacts built up over time, so while a modern company needs a public face, it has always been a job for mañana. Using something like Wix when you work in IT feels like cheating, but there is a bewildering number of options for creating and hosting websites on the market, making it hard to know where to start. However a recent client had a requirement to build a *living documentation* web site using a framework called __Hugo__, so this presented an opportunity to dive in and skill up, ___fast___, with a view to applying the approach to this company website.

Microsoft recently introduced __Aure Static Websites__, a free (in preview) lightwieght service that automatically builds and deploys web apps to Azure from a GitHub repo, making it free and easy to get started with hosting your own website.

![staic apps overview](/images/static-apps-overview.png)

> An interesting post for another day - what does Microsoft's purchase and embrace of GitHub mean for the future of Azure DevOps?

### Pre-requisites

1. Microsoft VSCode (or other IDE such as atom)
2. A GitHub account
3. An Azure account
4. [Install Hugo](https://gohugo.io/getting-started/installing/)

## Create the basic Hugo website

### Step 1: Crete a new site

Create a new site called *docs* in the current directory:

```bash
hugo new site docs
```

### Step 3: Add a theme

See [the hugo website](https://themes.gohugo.io) to find a theme to use. Tihs website uses the Ananke theme, based on the Hugo quickstart. 

Download your chosen theme from GitHub, add it to the site's `theme` directory, then add it to the site configuration file `config.toml`


```bash
cd docs
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
echo 'theme = "ananke"' >> config.toml
```

### Step 4: Add a blog page

Add a blogs section and a page with your first blog - the following command will create a new directory called `blogs`, with a file called `my-first-blog.md`

```bash
hugo new blogs/my-first-blog.md
```

Edit the file using [Markdown](https://https://www.markdownguide.org/cheat-sheet/). 

```markdown
---
title: "This is the title of my first blog"
date: 2020-06-16T21:39:53+01:00
draft: true
---
```

> Before publishing the site, change `draft: true` to false. Pages in draft are not published.

### Step 5: Preview locally

Preview the page by running the hugo server in [draft](https://gohugo.io/getting-started/usage/#draft-future-and-expired-content) mode:

```bash
hugo server -D

                   | EN
+------------------+----+
  Pages            | 10
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  3
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

Total in 11 ms
Watching for changes in /Users/ad/quickstart/{content,data,layouts,static,themes}
Watching for config changes in /Users/ad/quickstart/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

> Click on [http://localhost:1313/](http://localhost:1313/) to view the site locally.

![staic apps overview](/images/web-preview.png)

## Deploy in to Azure

__TODO__

Open in new tab:
https://agrimprasad.com/post/hugo-goldmark-markdown/

Sticky ToC
https://ma.ttias.be/adding-a-sticky-table-of-contents-in-hugo-to-posts/

Add pic to blogs