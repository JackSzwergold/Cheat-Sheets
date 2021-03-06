---
Title: Ruby
Description: A cheat sheet for Ruby related items.
Author: Jack Szwergold
Date: 2015-09-20
Robots: noindex,nofollow
Template: index
---

### Install required Ruby GEMs for the test web application install.

    sudo gem install rack sinatra bundler --no-rdoc --no-ri

### Launching a `config.ru` file manually.

First, go into the `ruby_test` directory:

    cd ~/ruby_test

Make sure there is a `config.ru` which has contents like this; this is for a simple Sinatra app:

    # config.ru
    
    # Set the required GEMs.
    require "sinatra"
    
    # Require the app.
    require './bin/app.rb'
    
    # Run the Sinatra app.
    run Sinatra::Application

Now run the applictation like this:

    rackup

For Sinatra a port can be specified with the `-p` flag like this which overrides the `config.ru` settings:

    rackup -p 3333

Or it could even be manually launched directly through ruby like this:

    ruby bin/app.rb
