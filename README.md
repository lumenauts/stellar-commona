# Lumenauts
An unofficial Stellar guide—-a resource for educational tools and community collaboration.

## Content Standard
Our community uses the [Commona content repo standard](https://github.com/commona-org/) to collaborate on content. Anyone can add to the guides, submit their blog post, add their project to the directory, or add a new content model by submitting a PR. When a maintainer merges a PR, the frontend is rebuilt with the newly added content.

## Guidelines
There are some basic guidelines that need to be followed when contributing:
- All pages should have links to supporting sources/documentation and additional resources
- No marketing or sponsored content
- No inappropriate content
- No ICOs

## Contributing to the Lumenauts Commona
### Markdown
All content is formatted as Markdown. Use this [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) to understand GitHub-flavored markdown. Each content file should end with `.md`.

### Images and Files
All images or hosted files should be added to the top-level `assets` folder. 

*Example Image Markdown*

```![example image](./assets/example-image.png)```

To move up directories from subfolders, use `../` instead of `./` for each level you need to move up. 

### Page Metadata
You can add titles, descriptions, etc. to an item via frontmatter. 

*Raw Example Frontmatter:*

```
---
title: Documenation Page Title
description: Description of this page's contents.
order: 20
---
```

**Note:** It's recommended to increment orders by 10 so that you can easily add new pages in later. For example, to add a page between a 10-ordered page and a 20-ordered page, you'd set the new pages's order to `15`.

### Content Model Metadata
Each content model type - documentation, blog posts, directory projects, etc - has its own frontmatter type, which is described in a content model's `metadata.md` file. To designate a content model's title and what page template a content model should use, add a `metadata.md` file to each model's folder with the metadata in frontmatter format. 

*Example Metadata.md File*

```
---
title: Projects
template: directory
active: true
---
```

`title`: The title of the content model for SEO and page title purposes.

`template`: The type of content model, which tells the host what page template to use when building the content in this content model. Content model template types:
- `documentation`
- `directory`
- `blog`

`active`: The toggle for adding this content model to the host's build process. If `true`, the host will build this content model and its contents. If `false`, the host will not display this content model on the frontend.