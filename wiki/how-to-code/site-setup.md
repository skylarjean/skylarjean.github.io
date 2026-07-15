---
title: Site Setup
date: 2026-07-14
description: A description of everything I needed to install and setup for this site to work
tags:
  - jekyll
  - markdown
  - github-pages
  - ruby
  - homebrew
importance: 5
abstract: "Leave empty if you don't want to feature an abstract. "
---

I started by following [this tutorial from Strikingloo][1], which then lead to a number of other tutorials and steps and deviations. This is a rough copy of what I did, so I have any hope of doing it again.

[1]: < https://strikingloo.github.io/personal-wiki-set-up> "Strikingloo Personal Wiki Setup"

Something about having a mac with some OS

## Forking Template

I made a fork of [Strikingloo's template][2].

[2]: https://github.com/StrikingLoo/Personal-Wiki-Site-Setup "Strinkingloo Site Template"

Name it *username.github.io*

## Install Relevant Software

This site is built with `Jekyll`, and our instructions say simply to "install `jekyll`. Turns out, this isn't as simple as it seems. We're following [this tutorial to install Jekyll][3].

[3]: https://jekyllrb.com/docs/installation/macos/ "Jekyll Installation"

### Install Homebrew
Checkout more about Homebrew [here][4].

[4]: https://brew.sh/ "Homebrew Homepage"

1. Check that mac software is up to date 
2. Install Homebrew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

### Install `chruby` and `ruby`
Jekyll is built on ruby, so we need ruby installed. Seemingly, the different versions of ruby are known to cause problems, so we need `chruby` to manage them. Different "packages" within `ruby` are called gems. We'll install some gems later.

There is a long tutorial about how to install `ruby` [here][5]. I didn't actually follow it, I just took a few nuggets.

[5]: https://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/ "Installing Ruby on Mac"

1. Install `chruby` and `ruby`: 
```
brew install chruby ruby-install
```
2. Install the latest version of `ruby`: 
```
ruby-install ruby 3.4.1
```
3. Configure your shell to use `chruby` (I use bash):
```
echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.bash_profile\
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.bash_profile
echo "chruby ruby-3.4.1" >> ~/.bash_profile
```

Relaunch the terminal to make sure that it sets up ruby correctly. You can run `ruby -v` or `chruby` to see that the version of ruby that is active.

You can also use `which ruby` to check that it's using the one you installed rather than the system one. (You really should install `chruby` and `ruby` as [opposed to using the one that comes installed on Mac][6].)

[6]: <https://www.moncefbelyamani.com/why-you-shouldn-t-use-the-system-ruby-to-install-gems-on-a-mac/> "Don't Use System Ruby on Mac"

To make sure the project uses the correct version of `ruby`, make a `.ruby_version` file with the version you have installed in it. That looks like
````
echo '3.4.1' >> .ruby-version
````

The next step is to have the `Gemfile` look at this `.ruby-version` file to get the `ruby` version. As far as I can tell, the `Gemfile` is a place that specifies versions for the gems you're using for a specific project. You can use the following line to read the `ruby` version into the `Gemfile`:

```
ruby File.read(".ruby-version").strip
```

### Install `Jekyll` (& Other Gems)

Now, we can install the gems we need for the site to compile.
```
gem install rails
gem install jekyll
gem install jekyll bundler
```

Note that the Strikingloo's tutorial use `sudo` in front of the gem installs; however other installs say to [never use `sudo` for gem installs][7].

[7]: https://www.moncefbelyamani.com/why-you-should-never-use-sudo-to-install-ruby-gems/ "Don't Use Sudo for Gem Installs"

##  Backend Setup

Once everything is installed, it should be easy to setup `jekyll` to compile everything. In the project directory, run
```
bundle init
bundle install
bundle add jekyll
```

## Frontend Setup

Once the backend is setup, we get to compile everything for the frontend! Run
```
bundle exec jekyll serve
```
You can then go to [http://localhost:4000](http://localhost:4000/) to view the site locally!

### Resolving Issues

If you're like me, that last step did not go as smoothly as you wanted it to. I also had to complete the following pieces to get everything running.

#### Install `csv` Gem

```
gem install csv
```

#### Add `csv` and `rails` Gems to `Gemfile`

```
gem "csv"
gem "rails"
```

#### Handling the Ugly `Liquid` Error

TL;DR: Specifiy the version of `liquid` in the `Gemfile`
```
gem "liquid", "~> 4.0.4"
```

Explanation:

I was running into this error of the form `Liquid Exception: undefined method tainted?`. There seems to be come backward compatibility issues between  `liquid`, `jekyll`, and `ruby`. There are lots of people [negotiating this on the forums][8].

[8]: https://talk.jekyllrb.com/t/liquid-4-0-3-tainted/7946/17 "Liquid Error Forum"

You can see what versions are required to work together in the `Gemfile.lock`. This file seems to be generated by something and is **not** meant to be edited by people. But is is a (mostly) human-readable list of dependencies etc. A the bottom of the file, it specifies which version of `liquid` this version of `jekyll` needs. Mine says "4.04", so that's why I use that version specification in the `Gemfile`. 

The bottom of `Gemfile.lock` also says what `ruby` and `bundler` version it's using, so that's nice too.

If you make changes to the `Gemfile`, you need to run a couple of commands to update the `Gemfile.lock` file.
```
bundle install
bundle update
```

## Host on Github Pages

This part I haven't done yet. We'll follow the Strikingloo tutorial when I get to it.
https://idratherbewriting.com/documentation-theme-jekyll/mydoc_publishing_github_pages.html


## Editing Everything

with obsidian!

## Tracking Everything

with git!

needed in the `.gitignore`:

```
_site/*
.jekyll-cache/*
.DS_Store
.obsidian/*
```

