# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal technical blog built with Hexo 7.3.0, hosted on GitHub Pages at https://zivyou.github.io. The blog contains 4,400+ posts and uses a custom forked theme (hexo-theme-polarbear) maintained as a Git submodule.

## Common Commands

```bash
# Install dependencies
npm install

# Create a new post
hexo new "Post Title"

# Start local development server
hexo server
# or
hexo s

# Generate static site (outputs to public/)
hexo generate
# or
hexo g

# Deploy to GitHub Pages (generates + deploys)
hexo deploy
# or
hexo d

# Clean generated files
hexo clean

# Combined generate and deploy
hexo g -d
```

## Project Architecture

### Directory Structure

- **source/_posts/** - All blog posts (Markdown files with front matter)
- **source/about/** - About page
- **source/todo/** - Todo page (skipped from rendering)
- **source/project/** - Project pages (skipped from rendering)
- **source/img/** - Images
- **themes/hexo-theme-polarbear/** - Active theme (Git submodule)
- **scaffolds/post.md** - Template for new posts
- **public/** - Generated static site (created by hexo generate)
- **_config.yml** - Main Hexo configuration

### Theme Architecture

The hexo-theme-polarbear theme uses:
- **SWIG templates** (not EJS) - Located in themes/hexo-theme-polarbear/layout/
- **Partial-based architecture** - Reusable components in _partial/ directory
- **Custom music player widget** - Embedded NetEase music player via widget_custom config

### Key Configuration Files

1. **_config.yml** (Main Hexo config)
   - Site metadata, URL settings
   - Permalink structure: `:year/:month/:day/:title/`
   - Theme selection: `hexo-theme-polarbear`
   - Deployment: Git deployer to master branch
   - MathJax enabled for LaTeX
   - Mermaid diagrams enabled
   - Skips rendering: `source/todo/**` and `source/project/**`

2. **themes/hexo-theme-polarbear/_config.yml** (Theme config)
   - Menu items: Archives, About, Project, Todo
   - Widget settings: Tags enabled, Categories disabled
   - Theme color scheme: Default (also supports Mint Green, Cobalt Blue, Hot Pink, Dark Violet)
   - Disqus comments enabled (shortname: zivyou)

### Content Front Matter Template

New posts use this scaffold (scaffolds/post.md):
```yaml
---
title: {{ title }}
date: {{ date }}
tags:
---
```

### Git Submodule Theme

The theme is a Git submodule tracking the `dev` branch of hexo-theme-polarbear:
- To update the theme: `cd themes/hexo-theme-polarbear && git pull dev`
- The theme is independently maintained in its own repository

### Multilingual Support

Configured for Chinese (zh-cn) and English with `i18n_dir: :lang`

### Special Features

- **post_asset_folder: true** - Each post can have an accompanying asset folder
- **Search functionality** - hexo-generator-search plugin generates search.json
- **Mermaid diagrams** - Flowcharts, sequence diagrams, etc. supported
- **MathJax** - LaTeX mathematical rendering
- **Tag cloud** - Custom color gradient (#BBBBEE to #337ab7)

### Deployment

- **Target**: GitHub Pages (zivyou/zivyou.github.com repository)
- **Branch**: master
- **Method**: hexo-deployer-git

### Development Workflow

1. Write posts in `source/_posts/` as Markdown files
2. Use `hexo server` to preview locally
3. Run `hexo generate` to build static site
4. Run `hexo deploy` to publish to GitHub Pages
