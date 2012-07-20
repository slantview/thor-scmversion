# Thor::SCMVersion

Thor tasks to manage a VERSION file based on SCM tags, for use in
continuous delivery pipelines.

One of the key points of continuous delivery is that every build has a
version number, and that version number is unique and only points to
one build. Thor::SCMVersion adds some tasks to a Thorfile to use SCM
tags as the authoritative source of version status, so your continuous
integration server can create versions without having to worry about
file conflicts.

## Integrating into your project

Since Thor is written in Ruby, you'll need Ruby to make this work, on
your workstation and on your CI server. See http://whatisthor.com/ for
more in how to use Thor.

### Get the gem

If you don't already have a Gemfile, you should probably get one. See
http://gembundler.com/ for more details on Bundler and Gemfiles. Not
required, but will save you a lot of hassle.

Add this line to your application's Gemfile:

    gem 'thor-scmversion'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install thor-scmversion

### Add it to your Thorfile

All you need to do is require it at the top of your Thorfile:

    require 'thor/scmversion'

Now when you list your thor tasks you'll see 2 new ones.

    $ thor list
    
    version
    -------
    thor version:bump TYPE  # Bump version number (type is major, minor or patch)
    thor version:current    # Show current SCM tagged version

### Remove your VERSION file from source control

Since your CI server will be managing your VERSION file, you don't
want it to be stored in your SCM anymore. Make a note of your current
version, then

    $ git rm VERSION
    $ echo VERSION >> .gitignore`
    $ git add .gitignore
    $ git commit -m "Drop VERSION as it is managed by thor-scmversion now"

### Create a tag for the current version

Now tag the current version manually. If it was 1.2.3:

    $ git tag 1.2.3
    $ git push --tags

You can make sure it worked by running `thor version:current` and
making sure it is what you expected.

### Integrate with CI

Now, as part of your CI job, before it builds your artifacts, have it
run `thor version:bump patch`. This will increment the patch, push the
new tag, and write the VERSION file. Now the artifact you build will
have the right version information, every time.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Contributors

* Michael Ivey <ivey@gweezlebur.com>
* Kyle Allan <kallan@riotgames.com>
* Josiah Kiehl <josiah@skirmisher.net>
* Based on code developed by Jamie Winsor <jamie@vialstudios.com>
* Originally derived from some Bundler internals by Yehuda Katz