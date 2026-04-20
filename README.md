# os-package-update-ans

An Ansible role that updates installed OS packages to their latest available versions.

## Current behavior

The role currently does the following:

- Gathers distribution facts.
- Verifies that the detected package manager is supported.
- Upgrades all packages on Linux hosts that use `apt`, `dnf`, or `yum`.
- Updates Homebrew metadata and upgrades installed formulae on Darwin hosts.

## Supported package managers

The role currently allows these package managers:

- `apt`
- `dnf`
- `yum`
- `homebrew`

## Tested platforms

CI runs Molecule scenarios for:

- Rocky Linux 8
- Rocky Linux 9
- Ubuntu 22.04
- Ubuntu 24.04

There is also a mixed Molecule scenario that runs the role against Rocky Linux 9 and Ubuntu 22.04 in the same test matrix.

## Requirements

Install the required collection before using the role:

```yaml
collections:
  - name: community.general
    version: 10.5.0
```

You can install it with:

```bash
ansible-galaxy collection install -r requirements.yml
```

## Role variables

Default variables:

```yaml
do_clean: false
```

Internal variables used by the role:

```yaml
supported_pkg_mgr:
  - apt
  - dnf
  - yum
  - homebrew
```

## Example playbook

```yaml
- name: Update system packages
  hosts: all
  become: true
  roles:
    - role: os-package-update-ans
```

## Notes and current limitations

- The role upgrades all packages to `latest`; it does not support selective package updates.
- Optional cleanup logic exists in `tasks/main.yml` but is currently commented out and inactive.
- Variables for supported operating systems and versions exist in the role, but they are not enforced by the current task logic.
- Galaxy metadata does not fully match current Molecule coverage.

## Development

Molecule scenarios are defined under `molecule/` and GitHub Actions runs:

- `default`
- `rocky-8`
- `rocky-9`
- `ubuntu-22`
- `ubuntu-24`
