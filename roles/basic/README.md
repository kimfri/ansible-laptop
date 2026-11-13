# basic

Grundroll för Fedora-laptopen. Installerar baspaket med `ansible.builtin.dnf`.

## Variabler

- `basic_terminator_package` – paketet som installeras, standard `terminator`.

## Exempel

```yaml
- name: Configure laptop
  hosts: laptops
  become: true
  roles:
    - basic
```
