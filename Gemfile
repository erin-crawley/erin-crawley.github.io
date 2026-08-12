source "https://rubygems.org"

# Jekyll 4, NOT the `github-pages` gem.
#
# The github-pages gem pins Jekyll 3.9, which pins Liquid 4.0.3, which calls
# String#tainted? — removed in Ruby 3.2. It cannot build on any current Ruby.
# Jekyll 4 uses Liquid 5 and works fine. The cost is that GitHub's built-in
# Pages builder can't be used, so the site deploys via GitHub Actions instead
# (.github/workflows/pages.yml). That also makes the build reproducible rather
# than dependent on whatever GitHub's legacy image happens to ship.
gem "jekyll", "~> 4.4"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-sitemap"
end

# Ruby 3.4+ dropped these from the default gems; Jekyll's dependency tree
# still expects them present.
gem "csv"
gem "base64"
gem "bigdecimal"
gem "logger"
gem "ostruct"

# Local `jekyll serve`.
gem "webrick"
