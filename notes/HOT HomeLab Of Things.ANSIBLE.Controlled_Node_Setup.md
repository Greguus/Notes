---
id: mybr5q8teq3cgw5295vo8le
title: Controlled_Node_Setup
desc: ''
updated: 1673952189637
created: 1673951621086
---

## Controlled Node Setup

Temporary draft (work in progress):

```yaml
---

- hosts: all
  tasks:
    - name: Add the user 'ansible'
      ansible.builtin.user:
        name: ansible
        comment: ansible
        shell: /bin/bash
        create_home: true
        state: present
    - name: Add user 'ansible' to sudoers
      lineinfile:
        path: /etc/sudoers
        regexp: '^ansible'
        line: 'ansible ALL=(ALL) NOPASSWD: ALL'
        validate: 'visudo -cf %s'
    - name: Remove 'authorized_keys' with root key
      become: yes
      ansible.builtin.file:
        path: /root/.ssh/authorized_keys
        state: absent
    - name: Set ssh key from file
      authorized_key:
        user: ansible
        state: present
        key: "{{ lookup('file', '/home/ansible/.ssh/id_rsa.pub') }}"
    - name: Set "PermitRootLogin" to no
      lineinfile:
        path: /etc/ssh/sshd_config
        regexp: '^PermitRootLogin'
        line: 'PermitRootLogin prohibit-password'
        state: present

```