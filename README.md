# ScottyLabs Blog

Because ScottyLabs needs a blog.

We're using a blogging framework called [Ghost](http://ghost.org). Read on to
learn how to get it set up for deployment or testing. There's also a discussion
of it's features and merits below.

## Deployment

You can deploy a production-ready version of this app using Docker.

First, get the source with Git:

```
git clone https://github.com/ScottyLabs/blog
git submodule init
git submodule update
```

### Linux

Follow these steps to get up and running. They assume you're working on Ubuntu.
If you're not, figure out how to install Docker and Fig on your own system.

```bash
# Install Docker, if you haven't already
curl -s https://get.docker.io/ubuntu/ | sudo sh

# If you have pip, you may want to install fig using:
#     sudo pip install -U fig
wget https://github.com/docker/fig/releases/download/1.0.1/fig-$(uname -s)-$(uname -m)
sudo mv fig-$(uname -s)-$(uname -m) /usr/local/bin/fig
chmod +x /usr/local/bin/fig
```

Then, anytime you want to start the blog server:

```
# Start the docker containers
fig up -d
```

### OS X

The steps to install on OS X are a little more complicated. Make sure you have
[Homebrew](http://brew.sh) installed, and then follow these instructions

```
# Install Docker, if you haven't already
brew update
# You can skip this step if you've already installed Homebrew Cask
# You can check this by running:    brew cask
brew install caskroom/cask/brew-cask
brew cask install virtualbox
brew install docker boot2docker

# If you have pip, you may want to install fig using:
#     sudo pip install -U fig
wget https://github.com/docker/fig/releases/download/1.0.1/fig-$(uname -s)-$(uname -m)
sudo mv fig-$(uname -s)-$(uname -m) /usr/local/bin/fig
chmod +x /usr/local/bin/fig
```

Then, anytime you want to start the blog server:

```
# Start the docker containers
fig up -d
```

## Testing

The Docker makes production deployment very easy, but it's not a good
environment for testing. To poke around and test things out, like themes, you're
going to want to install Ghost a different way. It's actually much easier than
using Docker:

```
# Checkout a copy of the Ghost source
git clone https://github.com/TryGhost/Ghost
git submodule init
git submodule update
```

Install node for your platform (again, assumes Ubuntu or Debian-related Linux
distro):

```
# -- LINUX --
sudo apt-get install node

# -- OS X --
brew install node
```

Then, build Ghost

```
npm install -g grunt-cli
npm install
grunt init
```

Finally, you can run Ghost with

```
npm start
```

If you're using this setup, chances are that you're going to want to write or
modify a theme. Be sure to [__read the documentation
here__](http://themes.ghost.org) for more information on how to write themes.
Writing your first theme for Ghost is probably best done by just copying someone
else's theme and modifying it.


## Project Layout

The `ghost/` directory contains all the things necessary to host a plain Ghost
blog, including the `Dockerfile` and the startup scripts. It's actually a
_submodule_, meaning that it's a Git repository within a Git repository. Someone
else maintains the scripts required to get up and running with Ghost on Docker,
and we're just including those scripts here for our project. If you make changes
in this directory, you won't be able to share them with others.

The `ghost-override/` directory contains all the files and assets that make our
deployment of the Ghost blog our own. This includes content like the posts
database, images uploaded through the web interface, and themes. The posts
database and images aren't tracked by the Git repository so that we don't
accidentally overwrite real posts between the testing and deployment phases.

Each custom theme should be in it's own directory within the
`ghost-override/content/themes/` folder. As an example, there's a theme there
right now named `crush`, so it's folder is
`ghost-override/content/themes/crush/`.

The `assets` directory contains any assets like images or other data that you
_do_ want to share.


## Features

Ghost is an excellent blogging platform. Here are some things it does really
well:

- Multiple authors
    - There is built in support for multiple authors, editors, and
      administrators. This means that everyone can work on their own content
      simultaneously without butting heads.
- Beautiful Markdown editing interface
    - Ghost features a side-by-side Markdown editing interface. Markdown is a
      markup language that simplifies the process of writing and formatting
      content for the web.
- Self-hosted
    - Because Ghost is open source, we have complete control over how our
      content is used. This makes migrating to a different platform easy if we
      ever decide to stop using Ghost. This also makes Ghost fun from an ops
      perspective.
- Easily theme-able
    - Ghost themes are written in Handlebars, one of the most popular templating
      frameworks.

## License

MIT License. See LICENSE
