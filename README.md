# NZBGet
NZBGet (nzbget.com) using maintained Debian release.

## [Requirements][i]
Requires [r_pufky.arr][g] galaxy-ng collection. See
[reference documentation][h] for troubleshooting and config variables.

Install size: ~11MB

Use absolute paths for directories in nzbget.conf. Default MainDir is
**/var/opt/nzbget**.

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage
Store all folders on fast, local drives (avoid network shares for QueueDir or
InterDir).

### [NZB Import Process][l]

1. NZBs added to NzbDir or API are picked up and queued.
2. Queue info is moved to QueueDir.
3. Articles are downloaded to the InterDir, then combined and verified (using
   PAR2 if needed).
4. RAR or 7z files are unpacked in the same directory (InterDir) after
   verification.
5. Once unpacking is complete, files are moved to DestDir — finished download
   location.
6. If enabled, scripts from ScriptDir run after completion (e.g. renaming,
   sorting, uploading).

### InterDir
Directory to store intermediate files.

Temporary working directory where all download processing, like repairs and
unpacking, takes place. After a job succeeds, the final files are moved to the
destination folder (DestDir), and the temporary files in InterDir are deleted.
DirectWrite uses this folder for post-processing.

Place InterDir on a fast disk (preferably SSD), and keep it separate from both
DestDir and the directory where Sonarr/Radarr transfers articles.

When you use a fast disk for InterDir, you eliminate disk writing
speed bottlenecks. By placing InterDir and DestDir on separate physical hard
drives, you also reduce disk wear.

Use local disk.

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag                   | Notes
 ------|------------------------|-------
  1    | nzbget_flg_apt_sources | Force required APT sources.
  2    | nzbget_flg_install     | Install nzbget.
  3    | nzbget_flg_config      | Deploy configuration files.
  4    | nzbget_flg_perms       | Set permissions based on nzbget.conf.

### Example Playbooks

#### New Deployments
[A minimal config][m] will be deployed if no configuration is specified. This
will deploy a working NZBGet install.

``` yaml
# Main directory: /var/opt/nzbget.
- name: 'NZBGet default install.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.nzbget'
```

#### Existing Deployments
Configuration and extension scripts are deployed as templates, allowing for
vault use of sensitive information. File permissions can be set based on the
configured directories in **nzbget.conf** to ensure the service can manage the
files.

``` yaml
- name: 'Set custom config, extension scripts, and ensure file permissions set.'
  ansible.builtin.include_role:
    name: 'r_pufky.arr.nzbget'
  vars:
    nzbget_flg_config: true
    nzbget_flg_perms: true
    nzbget_cfg_file: 'host_vars/nzb.example.com/config/nzbget.conf'
    nzbget_cfg_scripts_dir: 'host_vars/nzb.example.com/config/scripts'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

### [Releases][b]

 Release | Debian | Ansible | Notes
---------|--------|---------|-------
 5.x.x   | 13     | 2.20    | Ansible 2.20, feature flags, and semantic versioning.
 4.x.x   | 12     | 2.18    | Migrate to r_pufky.arr.
 3.x.x   | 13     | 2.18    | Migrate to Debian Trixie.
 2.x.x   | 12     | 2.18    | Data annotations.
 1.x.x   | 12     | 2.11    | Migration from private repository.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_nzbget/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_arr
[h]: https://r-pufky.github.io/docs/arr/nzbget
[i]: https://github.com/r-pufky/ansible_nzbget/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_nzbget/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_nzbget/blob/main/defaults/main/ports.yml
[l]: https://nzbget.com/documentation/nzbget-path-and-folder-structure-guide
[m]: https://github.com/r-pufky/ansible_nzbget/blob/main/files/default/nzbget.conf