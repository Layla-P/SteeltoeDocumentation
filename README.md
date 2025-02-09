# Documentation

## Overview

This is the home of Steeltoe documentation and blog articles. The site uses [DocFX](https://dotnet.github.io/docfx) to convert markdown to HTML, as well as site navigation. To get the DocFX CLI, visit [this](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html) page to see the different distributions.

If you are using [VS Code](https://code.visualstudio.com/) as your editor, you can use *most* of the features included in Microsoft's [docs authoring pack](https://marketplace.visualstudio.com/items?itemName=docsmsft.docs-authoring-pack).

## Folders

- /api: holds the project documentation markdown and table of contents
  - /v2: version 2 documentation
    - /all: this holds generated API docs from doc-comments in source code
  - /v3: version 3 documentation
    - /all: this holds generated API docs from doc-comments in source code
- /articles: holds the markdown for blog posts
- /images: the images
- /template/steeltoe: odd files that overwrite the default DocFX theme

## Markdown parser

DocFX offers a custom flavor of markdown with quite a few enhanced capabilities. To see examples and learn more, view the [DocFX Flavored Markdown specification](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html?tabs=tabid-1%2Ctabid-a).

### Links and Cross References

As you get familiar with DocFX, you'll notice the addition of a YAML header in the markdown files. Values in this header let you control page design, as well as set the page's `UID`. With this, you can create `xref` as well as use DocFX's `@` shorthand. Learn more about [linking in DocFX](https://dotnet.github.io/docfx/tutorial/links_and_cross_references.html).

**Note** it should be very rare that you hardcode a link to an 'HTML' page with your markdown. Instead, use its `UID` and let the path get calculated, as well as get links validated when building the project.

## Creating a new blog post

Create a new `.md` file in the `articles` folder. Name the file something authentic that is URL-safe. Then in `/articles/index.md`, include a shorthand link to the document, as well as a short description.

Here is a starter blog post:

```markdown
---
type: markdown
title: My Very Authentic Blog Post Title
description: A short description of my topic. Maybe 2 sentences long.
date: 01/01/2000
uid: articles/my-very-authentic-blog-post-title
tags: [ "modernize", 'something else", "and another thing" ]
author.name: Joe Montana
author.github: jmontana
author.twitter: thebigguy
---

# My Very Authentic Blog Post Title

Let's talk about something really cool...
```

## Creating a new API document

Similar to the blog post, you're going to create a new markdown file, but in the `api` folder. The name needs to be URL-safe. Notice in the api folder, there is a `v2` and `v3` subfolder. Within each of those are folders for each component. Place your content accordingly. To include the file in the table of contents, add it in `api/(version)/toc.yml`. Notice in the example below that the `topicHref` values are not absolute paths. DocFX will calculate everything at build time.

An example API doc:

```markdown
---
uid: api/v2/circuitbreaker/hystrix
---

# Netflix Hystrix

Steeltoe's Hystrix implementation lets application developers isolate and manage back-end dependencies so that a single failing dependency does not take down the entire application. This is accomplished by wrapping all calls to external dependencies in a `HystrixCommand`, which runs in its own...

Here is an example cross-reference link to config docs: @api/v2/configuration/cloud-foundry-provider
Or you could link to the v3 version of this doc: @api/v3/circuitbreaker/hystrix
Or do the same thing by providing custom link text: [view the v3 version](xref:api/v2/circuitbreaker/hystrix)
```

The corresponding entry in api/v2/toc.yml:

```yml
- name: Circuit Breakers
  items:
    - topicHref: circuitbreaker/hystrix.md
      name: Hystrix
```

## Page display options

In the YAML header of a page's markdown, you have options to turn page elements on or off. Below are those options.

|Yaml label  |Default value  |Description   |
|---------|---------|---------|
|_disableToc     |false|Turn off the left hand table of contents         |
|_disableAffix     |false|Turn off the right hand page navigation links         |
|_disableContribution     |false|Turn off right hand link to "edit this page"         |
|_disableFooter     |false|Don't show footer when guest scrolls to page bottom         |
|_enableSearch     |true|Show the search icon         |
|_enableNewTab     |true|All links on the page open in a new browser tab         |
|_disableNav     |false|Do not show top navigation links         |
|_hideTocVersionToggle|false     |Hide the version toggler in the table of contents         |
|_noindex     |false|Do not let search engines index the page         |
|_disableNavbar|false     |Do not show top bar of page         |

## Building the site

Use DocFX's [user manual](https://dotnet.github.io/docfx/tutorial/docfx.exe_user_manual.html) to build and run the site in a few different ways. The simplest way is to `cd` into the root folder of this project and run the following command. The site will build in a temporary folder named `_site` and be served at <http://localhost:8082>.

```powershell
docfx build --serve --port 8082
```

You can also specify where the build output should land

```powershell
docfx build -o "../publish"
```

## Base host address

By default, the navigation links will use the live site (<https://steeltoe.io>) as the base host address. You can override that by including the applicable metadata file.

If running the MainSite locally on port 8080, then use the `localhost.json` metadata file.

```powershell
docfx build --serve --port 8082 --globalMetadataFiles "localhost.json" --logLevel Warning
```

If running the MainSite locally with the dev site, then use the `devhost.json` metadata file.

```powershell
docfx build --serve --port 8082 --globalMetadataFiles "devhost.json" --logLevel Warning
```

## Updating Generated API Docs

The contents of the folder `/api/(version)/all` was generated by running `docfx metadata` against the core Steeltoe codebase.

## Running in Docker

To test the documentation locally in Docker, run the following command:

```bash
docker-compose up
```

The main site will be serving at <http://localhost:9081> and the documentation site will be serving at <http://localhost:9082>.
Note you can navigate from the navigation site to the documentation site using a browser.

## Contributions

All project information is available on the [Steeltoe Wiki](https://github.com/SteeltoeOSS/Steeltoe/wiki)
