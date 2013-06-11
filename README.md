Description
===========

This library enables easy rollout of changes on a per host basis. The
whitelist is stored in a data bag and can contain single hostnames or roles.
Hostnames are checked first and can contain glob patterns to enable similarly
named hostgroups. If none of the hostnames match, all roles in the `roles`
array are checked whether the host has one of them applied.

Requirements
============

Attributes
==========

Usage
=====

First create a data bag item under the `whitelist` data bag and give it a
descriptive name. You don't have to use the `whitelist` data bag but the
library defaults to that name. Then add hosts and roles you want to enable for
the rollout:

```
{
  "id": "my_whitelist",
  "patterns": [
    "host.example.com",
    "*.subdomain.example.com",
    "prefix*.example.com"
  ]
}

{
  "id": "my_whitelist",
  "patterns": [
    "host.example.com",
    "*.subdomain.example.com",
    "prefix*.example.com"
  ],
  "roles": [
    "Webserver",
    "DatabaseServer"
  ]
}
```

In your cookbooks you then have to add a dependency on the library cookbook in
`metadata.rb` like so:

```
depends "chef-whitelist"
```

Now all the recipes can branch depending on inclusion in a specific whitelist
with this simple statement:

```
if node.is_in_whitelist? "my_whitelist"
  # new hawtness
else
  # old stuff
end
```

If you don't use the default `whitelist` data bag and `patterns` key for
hostnames, you have to adapt the call to include the actual names:

```
node.is_in_whitelist? "my_whitelist", "whitelist_databag", "hostnames"
```

