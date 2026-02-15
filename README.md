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

See an [example workflow here](.github/workflows/playbook.yml), the verbosity
level is set to debug just to ensure no secrets are leaked in the logs.
