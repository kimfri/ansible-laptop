# ansible-laptop

Ansible-projekt för att provisionera den här Fedora-laptopen lokalt.

## Förkrav

- Fedora Linux med `dnf`
- Ansible:

  ```bash
  sudo dnf install -y ansible
  ```

  Detta är ett engångssteg. Projektet installerar inte Ansible självt.

## Användning

Kör från repots rot:

```bash
# Kontrollera syntax
ansible-playbook site.yml --syntax-check

# Torrkörning
ansible-playbook site.yml --check --diff --ask-become-pass

# Verkställ
ansible-playbook site.yml --ask-become-pass
```

`--ask-become-pass` kan utelämnas om sudo är lösenordslöst.

## Struktur

- `site.yml` – entrypoint som applicerar rollerna på gruppen `laptops`
- `inventory/hosts.yml` – `localhost` med `ansible_connection=local`
- `roles/basic/` – första rollen; installerar `terminator` med `ansible.builtin.dnf`

## Roller

### `basic`

Installerar grundpaket. Just nu endast `terminator`.

Variabel:

- `basic_terminator_package` – paketnamn, standard `terminator`
