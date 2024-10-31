source "https://rubygems.org"

# To update the latest github dependencies run: `bundle update`
# To list current versions: `budle exec github-pages versions`
# Check github versions: https://pages.github.com/versions/
gem "github-pages", group: :jekyll_plugins

group :jekyll_plugins do
  gem 'jekyll-feed'
  gem 'jekyll-paginate'
  gem 'jekyll-seo-tag'
  gem 'jekyll-sitemap'
end

install_if -> {RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end
