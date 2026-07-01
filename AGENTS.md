# AGENTS.md

## Cursor Cloud specific instructions

This repo is a single **Jekyll 3.9.2 static site** ("Wenhan's Music Player"). There is no
backend, database, or test suite — the only service is the Jekyll dev server. Standard
dev commands live in `README.md` (`bundle install`, `bundle exec jekyll serve`).

### Ruby version (important)
- The pinned stack (`jekyll 3.9.2` + `liquid 4.0.3` + `sass 3.7.4`) is **incompatible with
  Ruby 3.2+** because it relies on `Object#tainted?` and other APIs removed in Ruby 3.2.
- The environment therefore uses **Ruby 3.1.6 via `rbenv`** (`rbenv global 3.1.6`). `rbenv`
  is initialized in `~/.bashrc`, so interactive shells get the right `ruby`/`bundle`
  automatically. In non-interactive contexts, call the shim directly, e.g.
  `~/.rbenv/shims/bundle exec jekyll serve`.

### webrick
- `webrick` was removed from Ruby's stdlib in Ruby 3.0+, so `jekyll serve` needs it as an
  explicit gem. It has been added to the `Gemfile` (`gem 'webrick', '~> 1.8'`). Without it,
  the build works but `jekyll serve` crashes with `cannot load such file -- webrick`.

### Gems install location
- Bundler is configured (global `~/.bundle/config`) to install gems into `~/.bundle-gems`,
  **outside** the repo. Do NOT use an in-repo `vendor/bundle`: `_config.yml` overrides
  Jekyll's default `exclude` list, so a `vendor/` dir inside the repo gets scanned and
  breaks the build (`Invalid date ... welcome-to-jekyll.markdown.erb`).

### Running the site
- Dev server: `bundle exec jekyll serve --host 0.0.0.0 --port 4000 --livereload`
  (default port `4000`; LiveReload on `35729`).
- The only content page is the music player at `/ceye/`. `/` (root) is the redirect/index.
- The `Pagination ... couldn't find an index.html` warning at build time is harmless.

### External runtime dependencies
- The player page loads **jQuery from `ajax.aspnetcdn.com`** and streams audio/lyrics/covers
  from **`raw.githubusercontent.com/whwu17/resources/main`**. Playback requires outbound
  internet access; there is no local copy of the media assets.
