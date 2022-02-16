+++
title = "Hashicorp Consul"
draft = true
+++

Consul is a service that provides an alternative to extensive firewall configurations between nodes. It provides a DNS-like service for easy naming, and traffic gets routed through TLS encrypted proxies. It provides a key-value store for application wide settings. It's also an effective solution for networks you don't trust.


## Consul Template
An optional (?) service that can be used for generating configuration files, and updating them on the fly, based on settings from Consul and Vault.

Setup some templates (template-file:destination-file:optional-refresh-command)

```bash
consul-template \
    -template "/tmp/nginx.ctmpl:/var/nginx/nginx.conf:nginx -s reload" \
    -template "/tmp/redis.ctmpl:/var/redis/redis.conf:service redis restart" \
    -template "/tmp/haproxy.ctmpl:/var/haproxy/haproxy.conf"
```

Change a value.

```bash
consul kv put foo bar
```

And any templates that make reference to that value will get regenerated.


* <https://github.com/hashicorp/consul-template>
* <https://github.com/hashicorp/consul-template/blob/master/docs/configuration.md>


### Thoughts
It's my understand that vaults lock, meaning any vault changes would only be legal while the vault is unlocked. Mixing and matching Consul and Vault data might be a problem... but I don't exactly know how vault is implemented yet.


