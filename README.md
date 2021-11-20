dotfiles
=========

Copies dotfiles into place.  The dotfiles will be linked in the users homedir from a defined location. ( default is ~/Documents/dotfiles )

Requirements
------------

* A separate file for each dotfile needs to exist. The actual dotfiles need to be in a remote repository such as github or gitea.
* Needed to add the following to my playbook because I was getting ssl errors.  This bypassed it.  If using github to host teh dotfiles repo, this won't be needed.

```
- hosts: all
  gather_facts: yes
  environment:
     GIT_SSL_NO_VERIFY: 'true'
```

Role Variables
--------------

* **dotfiles_repo:** remote gitrepo where the dotfiles are kept
* **dotfiles_repo_version:** Name of the production branch for the repository where the dotfiles are kept.
* **dotfiles_repo_accept_hostkey:** automatically accept hostkey, or not
* **dotfiles_repo_local_destination:** Location of where the repo will be cloned to
* **dotfiles_home: "~"** Home directory of the user
* **dotfiles_homedirfiles:** list of files to be linked into home directory
* **dotfiles_aliasfiles_all:** list of the files where aliases are stored
* **dotfiles_aliases_norit:** all non-work aliases files go here
* **dotfiles_work_user:** work username
* **DotFiles:** Variable used in the role execution, when you specify users to apply against
  * **UserName:**  Nested Variable.  It's the username where the dotfiles will be configured to. 




Dependencies
------------

None

Usage
----------------

Playbook.yml
```
- hosts: all
  become: true
  gather_facts: yes
  environment:
     GIT_SSL_NO_VERIFY: 'true'
  roles:
    - role: dotfiles
      vars:
        DotFiles:
          - UserName: "vagrant"
          - UserName: "root"
```