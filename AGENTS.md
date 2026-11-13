# AGENTS.md

## Project

- Fedora-only Ansible project that provisions this laptop locally.
- Entrypoint is `site.yml`; the `laptops` group targets `localhost` with `ansible_connection=local`.
- Roles live in `roles/`. The first role is `basic`, which installs `terminator`, `fzf`, and `bash-git-prompt`, ensures their bash integration in `~/.bashrc`, sets `EDITOR=/usr/bin/vim`, and manages aliases from `basic_bash_aliases`.

## Commands

Run from the repo root:

- Bootstrap Ansible (already installed): `sudo dnf install -y ansible`
- Validate syntax: `ansible-playbook site.yml --syntax-check`
- Dry run: `ansible-playbook site.yml --check --diff --ask-become-pass`
- Apply: `ansible-playbook site.yml --ask-become-pass`
- Inspect targets: `ansible-inventory --graph`
- Verify the package: `rpm -q terminator`
- Verify bash-git-prompt: `git -C ~/.bash-git-prompt rev-parse HEAD`

`--ask-become-pass` is unnecessary when sudo is passwordless.

## Conventions

- Use `ansible.builtin.dnf`; this project targets Fedora only.
- Set `become: true` on individual privileged tasks, not at play level. User config tasks rely on `ansible_facts['user_dir']` resolving to the invoking user's home.
- Keep tasks idempotent. `terminator` and `fzf` are already installed, so a correct apply reports `ok`, not `changed`.
- Manage `~/.bashrc` with `lineinfile`, `replace`, or `blockinfile`; do not create duplicate fzf, EDITOR, alias, or bash-git-prompt entries.
- Add aliases to `basic_bash_aliases`; removing an entry does not remove its existing `.bashrc` line.
- `bash-git-prompt` is not a Fedora package. Install it with `ansible.builtin.git` and never update an existing clone (`update: false`).
- Prefix role-specific defaults with the role name (for example `basic_terminator_package`).
- Wire new roles into `site.yml`; do not add host-specific tasks there.
- `ansible-lint` is not installed and is not available in the Fedora 45 repos; do not assume it exists.

## Git

- Default branch is `main`; `origin` is `git@github.com:kimfri/ansible-laptop.git`.
- Commits are GPG-signed via the global git config (`commit.gpgsign=true`). Keep signing enabled; do not use `--no-gpg-sign`.
