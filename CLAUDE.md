# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands
- Build site: `bundle exec jekyll build`
- Dev server: `bundle exec jekyll serve`
- Create new post: `bundle exec jekyll compose "Post Title"`

## Code Style Guidelines
- **Markdown posts**: Follow Jekyll's naming convention `YYYY-MM-DD-title.md`
- **Front matter**: Include title, date, categories, and layout
- **Code snippets**: Use liquid tags with syntax highlighting `{% highlight language %}`
- **Images**: Store in `/assets/` directory with descriptive filenames
- **Links**: Use reference-style markdown links for better readability
- **Line length**: Keep lines under 80 characters where possible
- **Headings**: Use sentence case for headings (capitalize first word only)
- **Quotes**: Use proper typographic quotes ("",'') rather than straight quotes
- **Lists**: Use hyphens (-) for unordered lists

## Project Organization
- Blog posts go in `_posts/` directory
- Static assets belong in `assets/` directory
- Site configuration in `_config.yml`