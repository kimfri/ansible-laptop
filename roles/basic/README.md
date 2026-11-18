# basic

Grundroll för Fedora-laptopen. Installerar baspaket med `ansible.builtin.dnf`, inklusive `make`, klonar `bash-git-prompt`, installerar VS Code via `tasks/vscode.yml`, installerar Podman och podman-compose via `tasks/podman.yml`, ser till att integrationen för fzf och bash-git-prompt finns i `~/.bashrc` och lägger till bash-funktionen `h` via `tasks/bash_functions.yml`.

## Variabler

- `basic_terminator_package` – paketet som installeras, standard `terminator`.
- `basic_fzf_package` – paketet som installeras, standard `fzf`.
- `basic_make_package` – paketet som installeras, standard `make`.
- `basic_podman_packages` – paket som installeras för Podman, standard `podman` och `podman-compose`.
- `basic_bash_git_prompt_repo` – repot som klonas, standard `https://github.com/magicmonty/bash-git-prompt.git`.
- `basic_bash_git_prompt_version` – branch eller commit, standard `master`.
- `basic_bash_git_prompt_dest` – klonens mål, standard `~/.bash-git-prompt`.
- `basic_editor` – editor som sätts i `~/.bashrc`, standard `/usr/bin/vim`.
- `basic_code_dir` – katalogen som `h` går till, standard `~/code`.
- `basic_bash_aliases` – lista med alias, standard:

  ```yaml
  - name: l
    command: "ls -l"
  ```

## Exempel

```yaml
- name: Configure laptop
  hosts: laptops
  roles:
    - basic
```
