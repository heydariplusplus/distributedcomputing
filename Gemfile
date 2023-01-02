# frozen_string_literal: true

source "https://rubygems.org"
gemspec

gem "jekyll", ENV["JEKYLL_VERSION"] if ENV["JEKYLL_VERSION"]
gem "kramdown-parser-gfm" if ENV["JEKYLL_VERSION"] == "~> 3.9"

gem "webrick", "~> 1.7"

gem 'jekyll-spaceship', group: :jekyll_plugins

gem 'jekyll-sitemap'

group :jekyll_plugins do
    gem 'jekyll-katex'
  end

# gem 'execjs'

# gem 'therubyracer', :platform => :ruby
