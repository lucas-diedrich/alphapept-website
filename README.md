# alphapept website

This repository contains the source code for the alphapept proteomics software ecosystem website, built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

**This is a work-in-progress, the preliminary name is alphapept**


## Live Site

_To be announced_

## Site Structure

```
content/
├── _index.md              # Homepage
├── ecosystem/             # Ecosystem
│   ├── _index.md          # Ecosystem overview
│   ├── walkthrough.md     # (work in progress: More detailed description of individual components+interplay)
|   └──packages/           # Ignored
|        *.md              # More detailed description of individual packages
├── mission.md             # Mission and goals
├── community/             # Community, Contributors, Guidelines
│   ├── _index.md          #
│   ├── contributors.md    # Current and former contributors
│   ├── guidelines.md      # Code of conduct, Diversity/Equity/Inclusion etc.
|   └── join               # How to contribute
└── news/                 # News and blog posts
    ├── _index.md         # News overview
    └── *.md              # Individual news posts
```

## Local Development

### Prerequisites

- [Hugo](https://gohugo.io/installation/) (v0.121.0 or later)
- [Git](https://git-scm.com/)

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/alphapept-team/alphaX-website.git
   cd alphaX-website
   ```

2. **Initialize the theme submodule:**
   ```bash
   git submodule update --init --recursive
   ```

3. **Start the development server:**
   ```bash
   hugo server -D
   ```

4. **Open your browser** and navigate to `http://localhost:1313`

### Development Workflow

- **Content**: Edit Markdown files in the `content/` directory
- **Configuration**: Modify `hugo.toml` for site-wide settings
- **Layouts**: Custom layouts are in `layouts/` directory. Specifically, you can generate templates for specific structures (e.g. the Package/Contributor Cards and grids in `/layouts/shortcodes` with templated HTML+CSS)
- **Static files**: Images and other assets go in `static/` directory

## Adding Content

### New Package Documentation

To add a new package:

1. Create a new Markdown file in `content/packages/`
2. Add front matter with title, description, and date
3. Use the feature card shortcode on the packages index page

Example:
```markdown
---
title: "New Package"
description: "Description of the new package"
date: 2024-01-15
---
```

Ignore subsites (e.g. if you only want to be able to link to a specific site)

```markdown
---
...
_build:
  list: false
cascade:
  _build:
    list: false
---
```

Your content here...
```

### New News Post

To add a news post:

1. Create a new Markdown file in `content/news/` with date prefix
2. Add appropriate front matter and tags
3. The post will automatically appear in the news feed

Example:
```markdown
---
title: "Exciting News"
date: 2024-01-15T10:00:00Z
description: "Brief description"
tags: ["announcement", "release"]
---

Your news content here...
```

## Customization

### Shortcodes

The site includes custom shortcodes for consistent styling (see `/layouts/shortcodes`):

- **Feature Card**: `{{< feature-card title="Title" description="Description" url="/link/" icon="🔬" >}}`
- **Contributor Card**: `{{< contributor-card name="Name" affiliation="Affiliation" role="Role" github="URL" >}}`

### Theme Configuration

Key configuration options in `hugo.toml`:

- **Site metadata**: Title, description, author
- **Navigation menu**: Top-level menu items
- **PaperMod parameters**: Theme-specific settings
- **Social links**: GitHub, Twitter, etc.

## Deployment

### GitHub Pages (Automatic)

The site automatically deploys to GitHub Pages when changes are pushed to the `main` branch using GitHub Actions.

### Manual Deployment

To build the site manually:

```bash
hugo --minify
```

The generated site will be in the `public/` directory.


## Contributing

We welcome contributions to improve the website! Please:

1. **Fork** the repository
2. **Create** a feature branch
3. **Make** your changes
4. **Test** locally with `hugo server`
5. **Submit** a pull request

### Content Guidelines

- Use clear, concise language
- Include code examples where appropriate
- Add appropriate tags to news posts
- Optimize images for web use
- Follow the existing content structure

### Technical Guidelines

- Follow Hugo best practices
- Test locally before submitting PRs
- Use semantic HTML in custom layouts
- Ensure responsive design
- Maintain accessibility standards

## License

This website is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Support
- **Issues**: [GitHub Issues](https://github.com/MannLabs/alphaX-website/issues)

## Acknowledgments

- [Hugo](https://gohugo.io/) - Static site generator
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod) - Hugo theme
- [GitHub Pages](https://pages.github.com/) - Hosting platform
- All contributors who help improve the site

---

**Built with ❤️ by the alphapept community**
