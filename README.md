## Requirements

### External

1. Consul
    1. Must be installed/configured on the target hosts/inventory.
    1. ACL token for usage by Vault (see `example_environment.yml`).
        - The generated master token can be used in a pinch, and can be 
          acquierd directly from a consul server host, by running:
          ```sh
          cat /var/lib/consul/config.json | grep master
          ```
    2. Consul DNS: must be configured and working properly

### Internal

#### Host variables

This playbook expects a `datacenter` variable to be set on all hosts targeted,
such that Vault is aware of the topology.

See the test inventory file for an example!

#### Playbook Variables

Configure Consul ACL details as shown in `vars/example_environment.yml`.

The playbook will look for a credential file in `vars/` that matches the ansible
environment, *e.g.*:

- `production_environment.yml`
- `test_environment.yml`
- etc.

One way to secure these files is to encrypt them files with `ansible-vault`:

```sh
KWUXLAB_ENV="production"; ansible-vault encrypt \
  --vault-id password_file."${KWUXLAB_ENV}".txt \
  playbooks/kwuxlab_ansible_service_consul_acls/vars/"${KWUXLAB_ENV}"_environment.yml
```

Which requires that `password_file."${KWUXLAB_ENV}".txt`
(read: `password_file.production.txt` in the above example) exists in your
current directory.