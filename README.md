# Ecorp.dev

Dan’s blog at [ecorp.dev](https://ecorp.dev), built with Jekyll and the
[Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy).

## Local development

Install Ruby 3.4 (see `.ruby-version`) and Bundler, then run:

```sh
bundle install
bundle exec jekyll serve
```

Open <http://localhost:4000>. Posts live in `_posts`, images in `assets/img`,
and site settings in `_config.yml`. Use `/assets/img/...` paths for post images.

Before opening a pull request, run the production build and internal link checks:

```sh
bash tools/deploy.sh --dry-run
```

External websites are excluded from this check so third-party outages do not
block publication.

## Theme customizations

The theme supplies its own includes, translations, assets, and Sass. Keep site
styling in `assets/css/jekyll-theme-chirpy.scss` instead of copying the framework.
The two local layouts are based on Chirpy 7.6.0: `home.html` preserves the slash
prefixes on titles/categories, and `post.html` omits the author byline and keeps
the reading time. Compare these layouts with upstream when upgrading the theme.

## Deployment and maintenance

GitHub Actions builds and checks pull requests. After a merge to `main`, the
workflow builds and publishes to `gh-pages`, preserving the existing custom-domain
`CNAME`. GitHub Pages should continue to serve the root of that branch.
Only the deployment job has repository write permission. Do not run the deployment
script without `--dry-run` locally; its publishing mode replaces the checkout.

Dependabot proposes monthly updates for gems and GitHub Actions. Keep
`Gemfile.lock` committed so local and CI builds use the same dependencies.

```sh
bundle update
bash tools/deploy.sh --dry-run
```

This repository is licensed under [MIT](LICENSE).
