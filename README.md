# sprout-wrap

# THE IAD FORK!!!

Hey IAD friends. This fork is intended just to allow us to customize `soloistrc`. We should not be altering any other files in this repository.

The intent is that we pull regularly from the upstream master branch of `sprout-wrap`. This is a manual process until we write some tooling
to automate this (and thus delete this fork).

Reminder for IAD folks: DO NOT ATTEMPT TO FIX THINGS IN THIS REPOSITORY. We have the `sprout-iad` cookbook for adding new dependencies.

## Prerequisites

Download and install [XCode 7 from the App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)

## Installation

To provision your machine, open up Terminal and enter the following.

First off, make sure that your sudo timeout is sufficient to last an entire sprout run. Open your sudoers file using the `visudo` command (it's very important that you use this and NOT regular vim - `visudo` will lock the file and, more importantly, perform grammar checks on the file. If you bork the sudoers file, then you won't be able to sudo to repair it).

```sh
$ sudo visudo -f /etc/sudoers
```

And add this line:

```
Defaults    timestamp_timeout=25
```

This will set the timeout to *25 minutes*, which is our estimate for the time needed to run sprout. If you see failures in sprout due to write permission errors, it's likely due to the timeout window not being long enough to last an entire sprout run. 
 
 ```sh
sudo xcodebuild -license
xcode-select --install
mkdir workspace
cd workspace
git clone https://github.com/iad-dev/sprout-wrap.git
cd sprout-wrap
caffeinate ./sprout
```

The `caffeinate` command will keep your computer awake while installing; depending on your network connection, soloist can take from 10 minutes to 2 hours to complete.

## Resprout
You can run `resprout` from the terminal to re-run the sprout-wrap. You may need to install gems on the system Ruby in order to resprout the first time. If you run into trouble, try the following:
```
cd workspace/sprout-wrap
unalias sudo
sudo gem install bundler
bundle
resprout
```

Once you have successfully resprouted, close and re-open your terminal. This will replace the sudo alias and get you any updates.

## Problems?

### clang error

If you receive errors like this:

    clang: error: unknown argument: '-multiply_definedsuppress'

then try downgrading those errors like this:

    sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future bundle

### Command Line Tool Update Server

If you receive a message about the update server being unavailable and are on Mavericks, then you already have the command line tools.

## Customization

This project uses [soloist](https://github.com/mkocher/soloist) and [librarian-chef](https://github.com/applicationsonline/librarian-chef)
to run a subset of the recipes in sprout's cookbooks.

[Fork it](https://github.com/pivotal-sprout/sprout-wrap/fork) to 
customize its [attributes](http://docs.chef.io/attributes.html) in [soloistrc](/soloistrc) and the list of recipes 
you'd like to use for your team. You may also want to add other cookbooks to its [Cheffile](/Cheffile), perhaps one 
of the many [community cookbooks](https://supermarket.chef.io/cookbooks). By default it configures an OS X 
Mavericks workstation for Ruby development.

Finally, if you've never used Chef before - we highly recommend you buy &amp; watch [this excellent 17 minute screencast](http://railscasts.com/episodes/339-chef-solo-basics) by Ryan Bates. 

## Caveats

### Homebrew

- Homebrew cask has been [integrated](https://github.com/caskroom/homebrew-cask/pull/15381) with Homebrew proper. If you are experiencing problems installing casks and
  have an older installation of Homebrew, running `brew uninstall --force brew-cask; brew update` should fix things.
- If you are updating from an older version of sprout-wrap, your homebrew configuration in soloistrc might be under `node_attributes.sprout.homebrew.formulae`
  and `node_attributes.sprout.homebrew.casks`. These will need to be updated to `node_attributes.homebrew.formulas` (note the change from formulae to formulas)
  and `node_attributes.homebrew.casks`.

## Roadmap

See Pivotal Tracker: <https://www.pivotaltracker.com/s/projects/884116>

## Discussion List

  Join [sprout-users@googlegroups.com](https://groups.google.com/forum/#!forum/sprout-users) if you use Sprout.

## References

* Slides from @hiremaga's [lightning talk on Sprout](http://sprout-talk.cfapps.io/) at Pivotal Labs in June 2013
* [Railscast on chef-solo](http://railscasts.com/episodes/339-chef-solo-basics) by Ryan Bates (PAID)
