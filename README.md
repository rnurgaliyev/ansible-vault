# Ansible role for HashiCorp Vault

This is a simple yet very flexible role to install and configure [HashiCorp Vault](https://vaultproject.io/).

Major pros of this role:
* Simplicity
* Automatic detection of latest available Vault version
* Flexible configuration

## Example usage

```yaml
---

# vault_install controls if role should try to install vault.
# vault_install_version controls which version should be installed, and
# can be set to 'latest'. In this case, role will fetch latest vault version
# according to hashicorp software versioning REST API.
vault_install: True
vault_install_version: latest

# vault_configure controls if vault should be configured.
vault_configure: True

# vault_configuration is a list of vault configurations. Every item
# will be deployed to vault configuration directory as a separate json file.
# Content of configuration key will be translated to json.

vault_configuration:
  - name: server
    configuration:
      ui: True
      storage:
        consul:
          address: "127.0.0.1:8500"
          path: "vault/"
      listener:
        - tcp:
            address: "127.0.0.1:8200"
            tls_disable: 1
        - tcp:
            address: "{{ ansible_default_ipv4.address }}:8200"
            tls_disable: 1
      api_addr: "http://{{ ansible_default_ipv4.address }}:8200"
      cluster_addr: "http://{{ ansible_default_ipv4.address }}:8201"
```
