---
Title: Ruby - Sundry Ruby GEM Items
Description: Ruby - Sundry Ruby GEM Items
Author: Jack Szwergold
Date: 2015-09-20
Robots: noindex,nofollow
Template: index
---

### List all locally installed Ruby GEMs.

    gem query --local

### Installing Ruby GEMs.

Ruby 1.9/1.8 method of installing a specific version of a Ruby GEM:

    sudo gem install [gem name here] --no-rdoc --no-ri --version=2.15.5

Ruby 2.0 method of installing a specific version of a Ruby GEM:

    sudo gem install [gem name here]:2.15.5 --no-rdoc --no-ri

### Cleaning up old Ruby GEMs.

Will prompt you to choose which GEMs you want to remove.

    sudo gem uninstall [GEM name]

Will remove only version 1.1.9.

    sudo gem uninstall [GEM name] --version 1.1.9

Will remove all versions less than 1.3.4.

    sudo gem uninstall [GEM name] --version '<1.3.4'

Will remove all old versions of the GEM.

    sudo gem cleanup [GEM name]
