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
- `roles/basic/` – första rollen; installerar `terminator` och `fzf` samt konfigurerar fzf:s bash-integration

## Roller

### `basic`

Installerar grundpaket och användarkonfiguration. Just nu `terminator` och `fzf` samt fzf-integration i `~/.bashrc`.

Variabler:

- `basic_terminator_package` – paketnamn, standard `terminator`
- `basic_fzf_package` – paketnamn, standard `fzf`
