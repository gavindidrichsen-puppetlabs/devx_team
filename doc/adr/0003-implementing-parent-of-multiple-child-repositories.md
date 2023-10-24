# 3. implementing parent of multiple child repositories

Date: 2023-10-17

## Status

Accepted

## Context

The ``devx`` team needs to manage multiple repositorys.  Some are individual [tools](https://puppetlabs.github.io/content-and-tooling-team/tools/) and others are collections of repositories relating to processes like building and packaging the likes of the pdk.  How do we manage resources, like documentation and utilities, that span across the various repositories?  How might we collect the resources into logical groups like "tools" or "packaging", etc?

Some of these questions might be answered by collecting all the repositories beneath a parent repository that includes all related code and documentation.  Patrick Lee Scott's article ["Mono-repo or multi-repo? Why choose one, when you can have both?"](https://patrickleet.medium.com/mono-repo-or-multi-repo-why-choose-one-when-you-can-have-both-e9c77bd0c668) introduces some of the arguments around using such a parent or "multi-repo" vs a "mono-repo".  He also introduces a multi-repo management tool called [meta](https://github.com/mateodelnorte/meta#readme)

## Decision

Therefore, I decided to collect the ``devx`` team's repositories under a parent and manage it using [meta](https://github.com/mateodelnorte/meta#readme)

## Consequences

Some of the benefits are as follows:

* It's very easy to add, remove, refactor the structure of the child repositories.
* Actions can be easily applied across all the repositories in one go using ``meta exec``.  For example, the following output shows how the [ruby-config] PCT template was applied to all the child projects:

```bash
➜  pdk_release_management git:(development) meta exec "pcta new devx/ruby-config" 

/tmp/pdk_release_management:
+ pct --templatepath=/tmp/pdk_release_management/templates new devx/ruby-config
4:26PM INF Deployed: /tmp/pdk_release_management
4:26PM INF Deployed: /tmp/pdk_release_management/.bundle
4:26PM INF Deployed: /tmp/pdk_release_management/.bundle/config
4:26PM INF Deployed: /tmp/pdk_release_management/.envrc
4:26PM INF Deployed: /tmp/pdk_release_management/.ruby-version
+ set +x
/tmp/pdk_release_management ✓

repos/packaging/packaging:
+ pct --templatepath=/tmp/pdk_release_management/templates new devx/ruby-config
4:26PM INF Deployed: /tmp/pdk_release_management/repos/packaging/packaging
4:26PM INF Deployed: /tmp/pdk_release_management/repos/packaging/packaging/.bundle
4:26PM INF Deployed: /tmp/pdk_release_management/repos/packaging/packaging/.bundle/config
4:26PM INF Deployed: /tmp/pdk_release_management/repos/packaging/packaging/.envrc
4:26PM INF Deployed: /tmp/pdk_release_management/repos/packaging/packaging/.ruby-version
+ set +x
repos/packaging/packaging ✓

repos/vanagon/puppet-runtime:
+ pct --templatepath=/tmp/pdk_release_management/templates new devx/ruby-config
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/puppet-runtime
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/puppet-runtime/.bundle
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/puppet-runtime/.bundle/config
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/puppet-runtime/.envrc
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/puppet-runtime/.ruby-version
+ set +x
repos/vanagon/puppet-runtime ✓

repos/vanagon/vanagon:
+ pct --templatepath=/tmp/pdk_release_management/templates new devx/ruby-config
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/vanagon
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/vanagon/.bundle
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/vanagon/.bundle/config
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/vanagon/.envrc
4:26PM INF Deployed: /tmp/pdk_release_management/repos/vanagon/vanagon/.ruby-version
+ set +x
repos/vanagon/vanagon ✓
direnv: error /tmp/pdk_release_management/.envrc is blocked. Run `direnv allow` to approve its content                                    
➜  pdk_release_management git:(development) ✗ 
```
