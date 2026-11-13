# AGENTS.md

## Project

- Fedora-only Ansible project that provisions this laptop locally.
- Entrypoint is `site.yml`; the `laptops` group targets `localhost` with `ansible_connection=local`.
- Roles live in `roles/`. The first role is `basic`, which installs `terminator`, `fzf`, `bash-git-prompt`, and VS Code, ensures bash integration in `~/.bashrc`, sets `EDITOR=/usr/bin/vim`, manages aliases from `basic_bash_aliases`, and defines the `h` bash function.

## Commands

Run from the repo root:

- Bootstrap Ansible (already installed): `sudo dnf install -y ansible`
- Validate syntax: `ansible-playbook site.yml --syntax-check`
- Dry run: `ansible-playbook site.yml --check --diff --ask-become-pass`
- Apply: `ansible-playbook site.yml --ask-become-pass`
- Inspect targets: `ansible-inventory --graph`
- Verify the package: `rpm -q terminator`
- Verify VS Code: `rpm -q code`
- Verify bash-git-prompt: `git -C ~/.bash-git-prompt rev-parse HEAD`
- Verify bash function: `bash -ic 'type h'`

`--ask-become-pass` is unnecessary when sudo is passwordless.

## Conventions

- Use `ansible.builtin.dnf`; this project targets Fedora only.
- Set `become: true` on individual privileged tasks, not at play level. User config tasks rely on `ansible_facts['user_dir']` resolving to the invoking user's home.
- Keep tasks idempotent. `terminator` and `fzf` are already installed, so a correct apply reports `ok`, not `changed`.
- Manage `~/.bashrc` with `lineinfile`, `replace`, or `blockinfile`; do not create duplicate fzf, EDITOR, alias, or bash-git-prompt entries.
- Add aliases to `basic_bash_aliases`; removing an entry does not remove its existing `.bashrc` line.
- `bash-git-prompt` is not a Fedora package. Install it with `ansible.builtin.git` and never update an existing clone (`update: false`).
- VS Code is configured in `roles/basic/tasks/vscode.yml` using the Microsoft RPM repo, `rpm_key`, `yum_repository`, and `dnf`.
- Bash functions are configured in `roles/basic/tasks/bash_functions.yml` using the marked `bash_functions` block.
- Keep `import_tasks` calls grouped together after the package-install tasks in `roles/basic/tasks/main.yml`.
- Prefix role-specific defaults with the role name (for example `basic_terminator_package`).
- Wire new roles into `site.yml`; do not add host-specific tasks there.
- `ansible-lint` is not installed and is not available in the Fedora 45 repos; do not assume it exists.

## Git

- Default branch is `main`; `origin` is `git@github.com:kimfri/ansible-laptop.git`.
- Commits are GPG-signed via the global git config (`commit.gpgsign=true`). Keep signing enabled; do not use `--no-gpg-sign`.
