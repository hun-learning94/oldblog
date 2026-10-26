# Hun Learning — archived blog

Published website: https://hun-learning94.github.io/oldblog/

This repository contains both the published website and its original Hugo source.

## Layout

- The root HTML files, `posts/`, `tags/`, `categories/`, CSS, JavaScript,
  and image folders are the published static site. GitHub Pages serves them
  directly from the root of the `master` branch. `.nojekyll` disables processing.
- `hugo-source/` contains the original posts, static assets, archetypes, and
  Hugo configuration imported from
  https://github.com/hun-learning94/hunlearning_contents at commit `b5dfdaf9db1ed5e1271bfcac8269a277d4bcb0ee`.
  Its original Git history is retained through the import merge commit.

The separate `hunlearning_contents` repository is no longer needed to host or
retain the imported source. It has not been deleted or archived.

## Editing and rebuilding

Editing files in `hugo-source/` alone does not update the published blog.
The original source repository specifies the `mainroad` Hugo theme but does not
include it. A compatible theme installation is still required to rebuild;
the published HTML identifies its generator as Hugo 0.80.0. The exact historical
theme revision is not recorded, so a rebuild may differ from the archived site.

The imported configuration is preserved unchanged, including its old root base
URL. When rebuilding, override the base URL with
`https://hun-learning94.github.io/oldblog/` and check hard-coded root-relative
links in posts and configuration. Build into a separate temporary directory
and preview it before replacing published files. Keep `hugo-source/`, this
README, `.nojekyll`, and Git history when updating the generated site.
