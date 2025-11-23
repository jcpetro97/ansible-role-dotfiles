dotfiles
========

Manage user dotfiles and aliases by cloning a centralized Git repository and symlinking files to user home directories. This also assumes that all users included will be getting the files listed in all of the variables.

Requirements
------------

- **Git**: Must be installed on the target system.
- **ACL**: The `acl` package is recommended on the target system to allow Ansible to manage files as unprivileged users (`become_user`) without permission errors.

Role Variables
--------------

- **dotfiles_repo_url** : The HTTPS URL of the Git repository containing the dotfiles.
- **dotfiles_repo_version** [default: `main`]: The branch, tag, or commit hash to checkout.

- **dotfiles_users** [default: `[]`]: A list of users to manage. Each item can contain:
    - `username` (Required): The system username.
    - `group` (Optional): The user's primary group. Defaults to the OS default.
    - `home` (Optional): The user's home directory. Defaults to `/home/<username>`. **Required** for the `root` user.

- **dotfiles_files_common** : List of files in the repo root to link to `~/.<filename>` for all users.
- **dotfiles_files_extra** : Optional list of extra dotfiles to link (e.g., for specific groups of users).

- **dotfiles_aliases_common** : List of alias files in the repo root to link to `~/.aliases/<filename>`.
- **dotfiles_aliases_extra** : Optional list of extra alias files.

[defaults/main.yml](defaults/main.yml)

Dependencies
------------

None

Example Playbook
----------------

```yaml
---
- name: Configure Workstations
  hosts: workstations
  become: true
  vars:
    dotfiles_repo_url: "https://gitlab.example.com/ops/dotfiles.git"
    
    # Define standard files everyone gets
    dotfiles_files_common:
      - ".vimrc"
      - ".bashrc"

    # Define users
    dotfiles_users:
      # Standard User (Home defaults to /home/student)
      - username: "user1"
        group: "user1"
      
      # Developer (Gets extra aliases)
      - username: "developer"
        group: "wheel"
      
      # Root User (Must specify home)
      - username: "root"
        home: "/root"

    # Extra aliases for this specific playbook run
    dotfiles_aliases_extra:
      - "aliases_dev"

  roles:
    - role: dotfiles
```
Molecule Testing
----------------

This role includes a full Molecule test suite that simulates the deployment on Docker containers (Rocky Linux 9 and Ubuntu 24.04). The tests automatically handle the creation of a mock Git repository and a test user.

**Prerequisites:**
- Docker
- Podman ( if using podman to test )
- Molecule ( with the drivers for podman and/or docker )
- Ansible Lint

Running Tests - Docker

`molecule test`

Running Tests - Podman
**Need to add this yet**

License
-------
MIT

Author Information
------------------
John Petro - 11/22/2025 Refactored Original Code