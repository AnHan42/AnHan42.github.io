[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
![Workflow Status](https://github.com/AnHan42/AnHan42.github.io/actions/workflows/gh-pages.yml/badge.svg)

# Anika Hannemann, personal academic site

Source for my personal academic website, built with [Hugo](https://gohugo.io/)
and deployed automatically to GitHub Pages. You can find the live site at
[anhan42.github.io](https://anhan42.github.io/).

## Credit

This site is built on the [wowchemy](https://wowchemy.com/) academic Hugo
theme, via a template adapted by [Simon Gravelle](https://github.com/simongravelle/simongravelle.github.io),
which also includes custom CSS originally from [nickballousite](https://github.com/nballou).
I have further customised the content, layout, and styling for my own use.

## How to build locally

To build locally on your computer, type:

```bash
hugo server
```

## How to modify

After cloning this repository:

- add your own content in the [content](content/) folder,
- modify the custom CSS in [assets/scss/custom.scss](assets/scss/custom.scss),
- enter publications in [content/publications/](content/publications/).

## How to deploy

In Settings, Pages, select:

- Deploy from a branch as Source
- gh-pages, `/(root)` as Branch

Pushing to `main` triggers the GitHub Action in
[.github/workflows/gh-pages.yml](.github/workflows/gh-pages.yml), which builds
the site with Hugo and pushes the result to the `gh-pages` branch.
