# Ansible role selinux

Managing SElinux.

- [Ansible role selinux](#ansible-role-selinux)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure a package manager is available and configured with the correct package sources and repositories.
- Ensure privileged permissions are set for the user executing this role to:
  - Install packages.
  - Manage SElinux settings and configurations.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_selinux_required_package_list | A per distribution package list. |
| role_selinux_required_packages | Rendered list of distribution package list. |
| role_selinux_state | The enforcing state. |
| role_selinux_policy | The policy.  |
| role_selinux_relabel | Whether to relabel at boot. |
| role_selinux_reboot_required | Whether to reboot after changes are set.  |
| role_selinux_dummy | Whether to ignore specific tasks that could render as an error. |

## Example Playbook

```yaml
---
- name: Manage SElinux
  become: True
  gather_facts: True
  tasks:
    - import_role:
        name: ansible_role_selinux
      vars:
        role_selinux_required_package_list:
          RedHat_7:
            - libselinux-python
            - libsemanage-python
            - policycoreutils-python
            - selinux-policy
          RedHat_8:
            - python3-libselinux
            - python3-libsemanage
            - python3-policycoreutils
        role_selinux_required_packages: '{{ role_selinux_required_package_list[ansible_distribution + "_" + ansible_distribution_major_version] | default([]) }}'
        role_selinux_state: enforcing
        role_selinux_policy: targeted
        role_selinux_relabel: False
        role_selinux_reboot_required: True
        role_selinux_dummy: False

...
```

## Useful shell commands

N/A

## Additional documentation resources

N/A

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_selinux>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_selinux>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_selinux>
