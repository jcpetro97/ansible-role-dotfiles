dotfiles
=========

Copies dotfiles into place.  The files are copied based on the <filename>.yml file, and that file is executed via a import from main.yml.

```
example:  bashrc.yml copies the .bashrc into place
```

Requirements
------------

A separate file for each dotfile needs to exist.

Role Variables
--------------

| Parameter | Choices | Comments |
| --------- | ------- | -------- |
| `UserName`| | Username where config will be copied to|
| `Location` | Choices:<ul><li>home</li><li>work</li></ul> |Set for customizations I have for work or home.|
| `PrimaryGroup` | | You only need this if you want to set a group other than users| 
| `Bashrc` | Choices:<ul><li>yes</li>|Set to yes if you have any bashrc aliased to add, otherwise don't set the variable|
| `BashCompletion`| Choices:<ul><li>yes</li><li>no</li></ul> | set this if you want bash completion configured |



Dependencies
------------

None

Usage
----------------

Playbook.yml
```
- hosts: all
  become: true
  
  roles:
    - role: dotfiles
      vars:
        DotFiles:
          - UserName: "test"
          - UserName: "test1"
            Location: "Work"
          - UserName: "test2"
            HomeDir: "/opt/test2"
            PrimaryGroup: "test"
          - UserName: "root"
            HomeDir: "/root"
            PrimaryGroup: "root"  
            Bashrc:
              - alias: "alias some command that should be aliased"
        BashCompletion: "yes"
```