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

### Step 5: Set links to open in a new tab

Create a file at `layouts/_default/_markup/render-link.html` with the following content:

```html
<a href="{{ .Destination | safeURL }}"{{ with .Title}} title="{{ . }}"{{ end }}{{ if strings.HasPrefix .Destination "http" }} target="_blank"{{ end }}>{{ .Text }}</a>
```

### Step 6: Add Table of Contents

Hugo has ToC capability built in. Add `toc: true` to the __front matter__ of your page.md_

```markdown
---
title: "First steps in to the world of building a small company website"
date: 2020-06-16T21:39:53+01:00
draft: false
toc: true

---
```

#### Configure smooth scrolling

By default, when you click on a link in the ToC, it will jump straigh to the section. Enabling smooth scrolling makes the transition happen more smoothly:

First, add a new folder under static called ___css___ and create a file called ___custom.ccs___ inside.

```markdown
.
static
├── css
     └── custom.css
```

Add the following text to ___custom.css___

```css
html {
  scroll-behavior: smooth;
}
```

When you click on a link in the table of contents now, it will scroll smoothly.

Now add `custom_css = ["css/custom.css"]` in the _[Params]_ section of __config.toml__

#### Configure sticky table of contents

Configuring the table of contents to follow the user as they scroll down the page, rather than disappear to the top, significantly improves the navigation experience. Follow these steps to applly this change across all posts:

- Copy the layout file that contains the ToC definition from the themes folder to the layouts/partials folder (you can customise the original file, but it is not recommended)

`cp themes/ananke/layouts/partials/menu-contextual.html layouts/partials/menu-contextual.html`

Look for the line:

`<div class="bg-light-gray pa3 nested-copy-line-height nested-links">`

Change it to:

`<div class="sticky bg-light-gray pa3 nested-copy-line-height nested-links">`

Adding the word 'sticky' just makes it easier to reference in the css file. Add the following section to __custom.css__:

```css
div.sticky {
  position: sticky;
  top: 140px;
  align-self: start;
}
```

The menu should now travel with the page when you scroll (as it does on the page you're reading!)

### Step 7: Preview locally

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

### Apendix - all major file changes:

custom.css:

```css
html {
	scroll-behavior: smooth;
}
div.sticky {
	position: sticky;
    top: 140px;
	align-self: start;
}
```

config.toml:

```toml
baseURL = "https://www.everywhereitsolutions.com/"
languageCode = "en-us"
title = "Everywhere IT Solutions Ltd"
description = "Journey to the cloud"
theme = "ananke"

MetaDataFormat = "yaml"
DefaultContentLanguage = "en"
SectionPagesMenu = "main"
Paginate = 3 # this is set low for demonstrating with dummy content. Set to a higher number
googleAnalytics = ""
enableRobotsTXT = true

[sitemap]
  changefreq = "monthly"
  priority = 0.5
  filename = "sitemap.xml"

[params]
  favicon = ""
  site_logo = ""
  description = "Journey to the cloud"
  facebook = ""
  twitter = "https://twitter.com/azure"
  instagram = ""
  youtube = ""
  github = ""
  gitlab = ""
  linkedin = ""
  mastodon = ""
  slack = ""
  stackoverflow = ""
  rss = ""
  # choose a background color from any on this page: http://tachyons.io/docs/themes/skins/ and preface it with "bg-"
  background_color_class = "bg-blue"
  featured_image = "/images/gohugo-default-sample-hero-image.jpg"
  recent_posts_number = 2
  show_reading_time = true
  custom_css = ["css/custom.css"]
  ```

menu-contextual.html:

```html
{{/*
  Use Hugo's native Table of contents feature. You must set `toc: true` in your parameters for this to show.
  https://gohugo.io/content-management/toc/
*/}}

{{- if .Params.toc -}}
<div class="sticky bg-light-gray pa3 nested-copy-line-height nested-links">
  <p class="f5 b mb3">{{ i18n "whatsInThis" . }}</p>
      {{ .TableOfContents }}
  </div>
{{- end -}}

{{/*
  Use Hugo's native related content feature to pull in content that may have similar parameters, like tags. etc.
  https://gohugo.io/content-management/related/
*/}}

{{ $related := .Site.RegularPages.Related . | first 15 }}

{{ with $related }}
  <div class="sticky bg-light-gray pa3 nested-copy-line-height nested-links">
    <p class="f5 b mb3">{{ i18n "related" }}</p>
    <ul class="pa0 list">
	   {{ range . }}
	     <li  class="mb2">
          <a href="{{ .RelPermalink }}">
            {{- .Title -}}
          </a>
        </li>
	    {{ end }}
    </ul>
</div>
{{ end }}
```

## Deploy in to Azure

