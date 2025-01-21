+++
date = "2024-10-18T22:57:07+08:00"
draft = true
title = "Hugo File Management"
lastmod = "2024-10-18T15:53:29.842Z"
+++


{{% badge title="Reference Page Lists" style="info" color="green" icon="arrow-right" %}}Hugo{{% /badge %}}
- [Directory structure](https://gohugo.io/getting-started/directory-structure/#union-file-system)
- [RELEARN Directory structure](https://mcshelby.github.io/hugo-theme-relearn/configuration/sitemanagement/structure/index.html)

## Directory Structure of *HUGO*

### Overview

- Each Hugo project is a directory
- Each of the subdirectories contributes to the content, structure, behavior, or presentation of your site.
  - `assets` contains files such as images, CSS, Sass, JavaScript, and TypeScript
  - `data` contains data files that augment content, configuration, localization, and navigation
  - `layout` contains templates to transform content, data, and resources into a complete website
  - `public` contains the published website
  - `resources` contains cached output from Hugo’s asset pipelines, by default includes CSS and images
  - `static` contains files that will be copied to the public directory when you build your site
  - `themes` contains one or more themes, each in its own subdirectory

```
my-site/
├── archetypes/       <-- contains templates for new content
│   └── default.md
├── assets/       <-- contains global resources typically passed through an asset pipeline
├── content/       <-- contains the markup files (typically Markdown) and page resources
├── data/       <-- contains data files (JSON, TOML, YAML, or XML)
├── i18n/       <-- contains translation tables for multilingual sites
├── layouts/       <-- contains templates
├── public/       <-- created when you build your site
├── resources/    <-- created when you build your site
├── static/       <-- contains static files
├── themes/       <-- contains one or more themes
└── hugo.toml         <-- site configuration
```

### Union file system

- Hugo creates a union file system, allowing you to mount two or more directories to the same location.
- You can include the shared content when you build your site using mounts. In your site configuration (.toml):
- By default, the union system **puts your root directory on top of the Relearn theme directory**. Files in your root directory will replace theme files in the same location. For example, if you create a file at `layouts/partials/heading.html`, it will override the theme’s `themes/hugo-theme-relearn/layouts/partials/heading.html`.

```toml
[module]
  [[module.mounts]]
    source = 'content'
    target = 'content'
  [[module.mounts]]
    source = '/home/user/shared-content'
    target = 'content'
```





## Content Directory Structure of *RELEARN*

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
```
