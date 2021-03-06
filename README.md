<p align="center">
  <img src="https://raw.githubusercontent.com/cloudconfig/cloco-docs/master/source/images/logo.png" width="100" height="104" />
</p>

# cloco-docs

Welcome to cloco-docs, the documentation repo for the cloco API.  This is an opensource documentation repo and we welcome contributions from the cloco user community.

The cloco API documentation is published at [http://docs.cloco.io](http://docs.cloco.io).

We are keen to build clients to allow easier interaction with cloco more languages, so if you are interested in contributing to a cloco client library and hence to the cloco documentation then please get in contact.

The cloco client libraries are all available on [GitHub](https://github.com/cloudconfig).

This documentation is based on [Slate](https://github.com/lord/slate), and more information on how to work with the documentation can be found there.

Getting Started with cloco-docs
------------------------------

If you want to contribute to cloco-docs and want to get a local build up and running, please use the following instructions:

### Prerequisites

You're going to need:

 - **Linux or OS X** — Windows may work, but is unsupported.
 - **Ruby, version 2.0 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

1. Fork this repository on Github.
2. Clone *your forked repository* (not our original one) to your hard drive with `git clone https://github.com/YOURUSERNAME/slate.git`
3. `cd slate`
4. Initialize and start Slate. You can either do this locally, or with Vagrant:

```shell
# either run this to run locally
bundle install
bundle exec middleman build --clean
bundle exec middleman server

# OR run this to run with vagrant
vagrant up
```

You can now see the docs at http://localhost:4567. Whoa! That was fast!

Now that Slate is all set up on your machine, you'll probably want to learn more about [editing Slate markdown](https://github.com/lord/slate/wiki/Markdown-Syntax), or [how to publish your docs](https://github.com/lord/slate/wiki/Deploying-Slate).

If you'd prefer to use Docker, instructions are available [in the Slate wiki](https://github.com/lord/slate/wiki/Docker).

### Contributing

Now that you've forked our cloco-docs repo, you can make your changes locally and then submit a PR to get your changed merged back into the cloco codebase.  

[Submit an issue](https://github.com/cloudconfig/cloco-docs/issues) to the cloco-docs GitHub if you need any help. And, of course, feel free to submit pull requests with bug fixes or changes.
