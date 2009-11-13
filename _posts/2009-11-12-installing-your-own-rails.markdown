---
layout: post
---
so i spent the last hour or two trying to get my own rails installed so i could successfully run rails + devise
on ruby 1.9.1.  thanks to a ticket (and prolly more) i was un-so-happily prevented from being happy with rails
2.3.4 (they need to release 2.3.5 already.)  So? i set out to install rails 2.3.4.1 (yes, not official.)  I really
don't want to freeze rails to my app (bloat?) so this was the next best thing.  I just have to worry about upgrading
my app when 2.3.5 gets released.

The last place i looked for making this simple is really the first place i should have.  below is how to happily
install your own rails to tide you over until a new release is out.  I've been called terse so here is my terseness
in action.  Im basing my local release off the 2-3-stable branch, but you could do this with any branch you want.

    $ git clone git://github.com/rails/rails.git
    $ cd rails
    $ git co -b rel 2-3-stable

now we have our proper branch checked out to make our release.  execute the following to make a happy release with
proper dependency tracking and the such.

    $ rake package PKG_BUILD=1

the 1 is what gives us 2.3.4.1 (if this was 2.3.3.1 id probably want to pick 2.)  this takes my 2.33Ghz dualcore
MBP with 4G of ram about 10 minutes to complete, so be patient it will finish.  there is probably a more simple
way of doing the following but i just wanted to get this installed already.

The gem's are kind of spread out so i just gave the paths to install each of them:

    $ find rails -type f -name \*.gem
    rails/vendor/rails/actionmailer/pkg/actionmailer-2.3.4.1.gem
    rails/vendor/rails/actionpack/pkg/actionpack-2.3.4.1.gem
    rails/vendor/rails/activerecord/pkg/activerecord-2.3.4.1.gem
    rails/vendor/rails/activeresource/pkg/activeresource-2.3.4.1.gem
    rails/vendor/rails/activesupport/pkg/activesupport-2.3.4.1.gem
    railties/pkg/rails-2.3.4.1.gem

you will find all the packages in there own directory.  the following command will install the gems without
annoying you with warnings/errors of any kind.

    $ find rails railties -type f -name \*.gem | xargs gem install -f -l
