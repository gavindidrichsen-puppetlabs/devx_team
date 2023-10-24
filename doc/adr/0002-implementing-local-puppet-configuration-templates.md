# 2. Implementing local puppet configuration templates

Date: 2023-10-17

## Status

Accepted

## Context

Sometimes before beginning development on a repository a number of files need to be put in place before the work can be safely begun.  

For example, before beginning work on a ruby project it is wise to configure bundler as well as the ruby version against which the project must be built.  Although [Bundler](https://bundler.io) is a useful tool for managing dependencies, it can also do unexpected things if it's not configured just right.  For example, if you want your ruby gems to be installed locally and your ``BUNDLE_PATH`` is not set corretly, then bundler may install gems in the system gem directory. This can lead to permission issues and gem conflicts.  3 files can be used to tighten up the ruby configuration:

* (1) A `.bundle/config` as below ensures that gems are installed locally and not on the system path:

```yaml
---
BUNDLE_PATH: ".direnv/ruby/gems"
BUNDLE_GEMFILE: "Gemfile"
BUNDLE_BIN: ".direnv/bin"
```

* (2) A `.envrc` file as below ensures not only that `rbenv` is initialized but also that the binstubs beneath `.direnv/bin` are added to the local `$PATH`:

```bash
# initialize rbenv shims
eval "$(rbenv init - zsh)" 
# ensure BUNDLE_BIN (e.g., .direnv/bin) is on PATH to access bundle binstubs
PATH_add .direnv/bin
```

* (3) A `.ruby-version` containing something like `3.2.0` ensures that ruby 3.2.0 is used.

What's the best way to ensure that the above 3 files are always added to a ruby project?  One solution is to create scaffolding that can be easily applied to a directory when required.

## Decision

Thereforem I created ruby config content scaffolding using [Puppet Content Templates](https://puppetlabs.github.io/devx/pct/) or `PCT`.  See the <https://github.com/gavindidrichsen-puppetlabs/pct_devx.git> which is included for more information.

## Consequences

Scaffolding can simplify complex configuration.  

For example, the following `ruby-config` template files encapsulate the ruby configuration described above.

```bash
➜  pdk_release_management git:(development) ✗ find templates -type f
templates/devx/ruby-config/0.1.0/content/.envrc
templates/devx/ruby-config/0.1.0/content/.ruby-version
templates/devx/ruby-config/0.1.0/content/.bundle/config
templates/devx/ruby-config/0.1.0/pct-config.yml
➜  pdk_release_management git:(development) ✗ 
```

To use the `ruby-config` scaffold:

* `export TEMPLATEPATH=<repo_root_directory>/templates`
* cd to the directory beneath which you want to add ruby configuration
* run either ``pct --templatepath=$TEMPLATEPATH new devx/ruby-config`` (or ``pcta new devx/ruby-config`` where ``pcta`` is an alias that injects the ``--templatepath`` and found beneath `bin/aliases`)
