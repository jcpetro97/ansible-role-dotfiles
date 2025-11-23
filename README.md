dotfiles
========

Manage user dotfiles and aliases by cloning a centralized Git repository and symlinking files to user home directories.

Requirements
------------

- **Git**: Must be installed on the target system.
- **ACL**: The `acl` package is recommended on the target system to allow Ansible to manage files as unprivileged users (`become_user`) without permission errors.
- **Privilege Escalation**: The role requires `become: true` to manage users and filesystem permissions.

Role Variables
--------------

- **dotfiles_repo_url** [default: `https://github.com/your-org/dotfiles.git`]: The HTTPS URL of the Git repository containing the dotfiles.
- **dotfiles_repo_version** [default: `main`]: The branch, tag, or commit hash to checkout.

- **dotfiles_users** [default: `[]`]: A list of users to manage. Each item can contain:
    - `username` (Required): The system username.
    - `group` (Optional): The user's primary group. Defaults to the OS default.
    - `home` (Optional): The user's home directory. Defaults to `/home/<username>`. **Required** for the `root` user.

- **dotfiles_files_common** [default: `['vimrc', 'gitconfig', 'bashrc']`]: List of files in the repo root to link to `~/.<filename>` for all users.
- **dotfiles_files_extra** [default: `[]`]: Optional list of extra dotfiles to link (e.g., for specific groups of users).

- **dotfiles_aliases_common** [default: `['aliases_common']`]: List of alias files in the repo root to link to `~/.aliases/<filename>`.
- **dotfiles_aliases_extra** [default: `[]`]: Optional list of extra alias files.

[defaults/main.yml](defaults/main.yml)

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
---
- name: Configure Workstations
  hosts: workstations
  become: true
  vars:
    dotfiles_repo_url: "[https://gitlab.example.com/ops/dotfiles.git](https://gitlab.example.com/ops/dotfiles.git)"
    
    # Define standard files everyone gets
    dotfiles_files_common:
      - "vimrc"
      - "bashrc"
      - "inputrc"

    # Define users
    dotfiles_users:
      # Standard User (Home defaults to /home/student)
      - username: "student"
        group: "students"
      
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
This role includes a full Molecule test suite that simulates the deployment on Docker containers (Rocky Linux 9 and Ubuntu 22.04). The tests automatically handle the creation of a mock Git repository and a test user.

Prerequisites
Docker

Molecule (pip install molecule molecule-docker)

Ansible Lint (pip install ansible-lint)

Running Tests
To run the full test sequence (lint, destroy, dependency, create, prepare, converge, verify, destroy):

`molecule test`

Test Logic
Prepare: Installs dependencies (git, acl, sudo), fixes PAM configuration for containers, creates a test user test_usr, and initializes a local "mock" Git repository in /tmp/mock_dotfiles.

Converge: Runs the role against the container, pointing it to the local mock repo instead of an external URL.

Verify: Checks that symlinks were created correctly in /home/test_usr and point to the expected targets inside .dotfiles/.

License
MIT

Author Information
------------------
John Petro - 11/22/2025 Refactored Original Code