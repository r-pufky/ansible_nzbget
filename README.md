# NZBGet
NZBGet (nzbget.com) using maintained debain release.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_nzbget/blob/main/meta/main.yml)

[collections/roles](https://github.com/r-pufky/ansible_nzbget/blob/main/meta/requirements.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_nzbget/tree/main/defaults/main)

## Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_nzbget/blob/main/defaults/main/ports.yml)

## Dependencies
Part of the [r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv)
collection.

## Example Playbook
Debian contrib, non-free APT sources are required and should be manually
managed if `nzbget_service_auto_add_apt_sources_enable: false`. Directories
will automatically be created and permissions set if needed.

host_vars/plex.example.com/vars/plex.yml
``` yaml
nzbget_main_dir: '/data/nzbget'
nzbget_control_username: '{{ vault_nzbget_user }}'
nzbget_control_password: '{{ vault_nzbget_pass }}'
nzbget_authorized_ip:
  - '0.0.0.0'
  - '127.0.0.1'
nzbget_news_servers:
  - id: 1
    name: 'example server'
    srv_hos: 'nzb.example.com'
    srv_port: 119
    username:'{{ vault_nzbget_news_user }}'
    password:'{{ vault_nzbget_news_pass }}'
```

Configure NZBGet server.
``` yaml
- name: 'Manage NZBGet'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.nzbget'
```

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_nzbget/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
