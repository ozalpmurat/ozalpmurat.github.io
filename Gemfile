# frozen_string_literal: true

source "https://rubygems.org"
#ruby "3.1"

gem "jekyll-theme-chirpy", "~> 5.0", ">= 5.0.2"
#gem 'jekyll-seo-tag'
#gem 'jekyll-sitemap'

group :test do
  gem "html-proofer", "~> 3.18"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :install_if => Gem.win_platform?

# Jekyll <= 4.2.0 compatibility with Ruby 3.0
gem "webrick", "~> 1.7"

gem "jekyll", "~> 4.3"
gem "tzinfo"
gem "csv"
gem "base64"
gem "bigdecimal"
