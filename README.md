# ansible-github-secrets

An Ansible module to manage GitHub secrets.

## Example Usage

### Playbook

```yaml
- name: Add Github secret
  github_secrets:
    token: "{{ lookup('env', 'GITHUB_TOKEN') }}"
    repository: "ansible"
    organization: "ansible"
    key: "TEST_SECRET"
    value: "bob"
    state: "present"

- name: Delete Github secret
  github_secrets:
    token: "{{ lookup('env', 'GITHUB_TOKEN') }}"
    repository: "ansible"
    organization: "ansible"
    key: "TEST_SECRET"
    state: "absent"
```

### GitHub action

```yaml
---
- name: Test playbook
  hosts: localhost
  any_errors_fatal: true
  gather_facts: false
  tasks:
    - name: Add Github secret
      github_secrets:
        token: "{{ lookup('env', 'ANSIBLE_TOKEN') }}"
        repository: "{{ lookup('ansible.builtin.env', 'GITHUB_REPOSITORY').split('/')[1] }}"
        organization: "{{ lookup('env', 'GITHUB_REPOSITORY_OWNER') }}"
        key: "TEST_SECRET"
        value: "bob"
        state: "present"
      register: add_result

    - name: Debug add result
      ansible.builtin.debug:
        var: add_result

    - name: Delete Github secret
      github_secrets:
        token: "{{ lookup('env', 'ANSIBLE_TOKEN') }}"
        repository: "{{ lookup('ansible.builtin.env', 'GITHUB_REPOSITORY').split('/')[1] }}"
        organization: "{{ lookup('env', 'GITHUB_REPOSITORY_OWNER') }}"
        key: "TEST_SECRET"
        state: "absent"
      register: delete_result

    - name: Debug delete result
      ansible.builtin.debug:
        var: delete_result
```

See an [example workflow here](.github/workflows/playbook.yml), the verbosity
level is set to debug just to ensure no secrets are leaked in the logs.
