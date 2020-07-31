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
| `Bashrc` | Choices:<ul><li>yes</li>|Set to yes if you have any bashrc aliased to add, otherwise don't set the variable|



Dependencies
------------

None

Usage
----------------

Playbook.yml
```
- hosts:  all
  become: true
  roles:
    - dotfiles
      vars:
        UserName: "testuser"
        Location: "home"
        Bashrc: "yes"
```