+++
date = "2024-10-18T22:27:18+08:00"
draft = true
title = "Hugo Quick Notes"
lastmod = "2024-10-18T15:39:59.743Z"
+++

## Create Pages

```
# Create Chapter Pages
hugo new --kind chapter first-chapter/_index.md
# Create Context Pages (3 ways)
hugo new first-chapter/first-page/_index.md
hugo new first-chapter/second-page/index.md
hugo new first-chapter/third-page.md
```

## Test and Deploy

- During test on [**http://localhost:1313**](http://localhost:1313), the browser will update automatically when you edit.
- The **public** directory can be uploaded to any web server, or use one of [Hugo’s many other ways to publish](https://gohugo.io/hosting-and-deployment/).


```
# Open http://localhost:1313 in your web browser to test on local
hugo serve
# Display drafts
hugo serve -**D**
# Creates a `public` directory
hugo
```

## Using *Front Matter CMS*

- CMS means "content management system"
- The theme supports the great [vscode Front Matter CMS extension](https://github.com/estruyf/vscode-front-matter) which provides on-premise CMS capabilities to Hugo. Put the following codes in `frontmatter.json` works.

```json
{
  "frontMatter.extends": [
     "themes\\hugo-theme-relearn\\vscode-frontmatter\\snippets.en.json"
  ]
}
```

## Directory Structure of *RELEARN*

- Each Hugo project is a directory

```
my-site/
├── archetypes/
│   └── default.md
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
├── themes/
└── hugo.toml         <-- site configuration
```

```
├── content
│   ├── first-chapter
│   │   ├── first-page
|   |   |   └── _index.md
│   │   ├── second-page
|   |   |   └── index.md
│   │   └── third-page.md
│   └── _index.md
├── themes
│   └── hugo-theme-relearn
│       └── ...
└── hugo.toml
```
