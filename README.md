# NZBGet
NZBGet (nzbget.com) using maintained Debian release.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_nzbget/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_nzbget/tree/main/defaults/main)

## Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_nzbget/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.arr](https://github.com/r-pufky/ansible_collection_arr) collection.

## Example Playbook
Debian contrib, non-free APT sources are required and should be manually
managed if `nzbget_srv_auto_add_apt_sources_enable: false`. Directories
will automatically be created and permissions set if needed.

host_vars/nzbget.example.com/vars/nzbget.yml
``` yaml
nzbget_cfg_main_dir: '/data/nzbget'
nzbget_cfg_control_username: '{{ vault_nzbget_srv_user }}'
nzbget_cfg_control_password: '{{ vault_nzbget_pass }}'
nzbget_cfg_authorized_ip:
  - '0.0.0.0'
  - '127.0.0.1'
nzbget_cfg_news_servers:
  - id: 1
    name: 'example server'
    srv_hos: 'nzb.example.com'
    srv_port: 119
    username: '{{ vault_nzbget_news_user }}'
    password: '{{ vault_nzbget_news_pass }}'
```

Configure NZBGet server.
``` yaml
- name: 'Manage NZBGet'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.nzbget'
```

Changes updating the configuration only can be done to speed role application:
``` bash
ansible-playbook site.yml --tags NZBGet \
  -e '{"nzbget_srv_force_config_only_enable": true}'
```

## Development
Configure [environment](https://r-pufky.github.io/ansible_collection_docs/ansible/environment)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Release format: **{OS}-{SERVICE}-{ROLE}**

Each type inherits the versioning system used; defaulting to schematic
versioning.

`12-2.0.3-1.0.0`

* 12 - Debian 12 (bookworm).
* 2.0.3 - Service/app version.
* 1.0.0 - Role version.

Releases are branched on Debian releases:

* **[13.x.x](https://github.com/r-pufky/ansible_nzbget)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_nzbget/tree/12.x)**: 12 Bookworm.

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_nzbget/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
