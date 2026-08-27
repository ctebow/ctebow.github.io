source "https://rubygems.org"

# Run the site locally with:  bundle exec jekyll serve --livereload
gem "jekyll", "~> 4.4.1"

# Theme. Upgrade with:  bundle update no-style-please
gem "no-style-please"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"   # /feed.xml for the writing section
  gem "jekyll-seo-tag"           # <title>, OpenGraph, JSON-LD
  gem "jekyll-sitemap"           # /sitemap.xml for search engines
end

# Windows and JRuby do not include zoneinfo files.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
