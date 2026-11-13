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
- `roles/basic/` – första rollen; installerar `terminator`, `fzf`, `bash-git-prompt` och VS Code samt konfigurerar fzf, bash-git-prompt och bash-funktioner i `~/.bashrc`

## Roller

### `basic`

Installerar grundpaket och användarkonfiguration. Just nu `terminator`, `fzf`, `bash-git-prompt` och VS Code samt integration för fzf och bash-git-prompt i `~/.bashrc` och funktionen `h`.

Variabler:

- `basic_terminator_package` – paketnamn, standard `terminator`
- `basic_fzf_package` – paketnamn, standard `fzf`
- `basic_bash_git_prompt_repo` – git-repo, standard `https://github.com/magicmonty/bash-git-prompt.git`
- `basic_bash_git_prompt_version` – branch eller commit, standard `master`
- `basic_bash_git_prompt_dest` – klonens mål, standard `~/.bash-git-prompt`
- `basic_editor` – editor som sätts i `~/.bashrc`, standard `/usr/bin/vim`
- `basic_code_dir` – katalogen som bash-funktionen `h` går till, standard `~/code`
- `basic_bash_aliases` – lista med alias (`name` och `command`), standard `l` → `ls -l`
