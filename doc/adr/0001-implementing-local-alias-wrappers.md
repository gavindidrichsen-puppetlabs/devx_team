# 1. Implementing local alias wrappers

Date: 2023-10-17

## Status

Accepted

## Context

I want to simplify common project actions and ease development and aliases can help achieve this.  Although, the `alias` command is useful for setting up simple alias definitions on Linux like `alias ls='ls --color'`; it is not the best approach when a wrapper script is more appropriate: for example, you may want your alias to do some setup, then call the target utility, then perform some tear down.  Further, tools like ``direnv`` don't work with ``alias`` definitions but work fine adding a directory of wrapper scripts to the `$PATH`.

A well designed alias can simplify complex utility command actions.  For example, ``be`` is an alias for the "bundle exec" that prefixes most commands when doing ruby development.

## Decision

Therefore, I decided to add alias wrapper script beneath `bin/aliases`. These aliases can either be added to the `$PATH` via a user's terminal profile or, alternatively if `direnv` is configured then `bin/aliases` can be added to the `$PATH` automatically via ``PATH_add bin/aliases`` in a `.envrc` file.

## Consequences

Not only do aliases simplify commands, but they can be updated easily when requirements change.
