# Theme

This website is based on the Jekyll theme https://mmistakes.github.io.

# Install for the first time

Requires Homebrew on your mac. You need to brew install openssl.

This uses Jekyll which is wonderful, but unfortunately requires Ruby on a mac, which is not.

Install the xcode command line tools for your flavour of MacOS from: https://developer.apple.com/download/more/

Install rvm:

    curl -sSL https://get.rvm.io | bash -s

Install ruby:

    bash # to get your newly installed rvm
    rvm install 3.2.2

Then add a few more dependencies:

    gem install eventmachine -- --with-openssl-dir=/usr/local/opt/openssl@1.1
    bundle add webrick

# Develop

Get the site:

    git clone https://github.com/sparrowfabrications.github.io
    cd sparrowfabrications.github.io
    bundle install

Then serve:

    bundle exec jekyll serve

# Update

From time to time it is worth doing:

    bundle update

Then committing any changes to Gemfile.lock .

##

You can also install chruby to manage ruby versions. Once you have, in your terminal run:

  chruby 3.4.9

to set a version. Then delete the Gemfile.lock and run

  ssh-add ~/.ssh/git/id
  git pull origin main
  bundler update --bundler
  bundler update --all

to update your dependencies and addres vulnerabilities.

To commit:

  ssh-add ~/.ssh/git/id
  git commit -a -m "Updated dependencies"
  git push origin main


