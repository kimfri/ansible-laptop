# basic

Grundroll för Fedora-laptopen. Installerar baspaket med `ansible.builtin.dnf` och ser till att fzf:s bash-integration finns i `~/.bashrc`.

## Variabler

- `basic_terminator_package` – paketet som installeras, standard `terminator`.
- `basic_fzf_package` – paketet som installeras, standard `fzf`.

## Exempel

```yaml
- name: Configure laptop
  hosts: laptops
  become: true
  roles:
    - basic
```
