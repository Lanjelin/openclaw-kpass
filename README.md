# openclaw-kpass

An [OpenClaw skill](https://docs.openclaw.ai/tools/skills) for working with KeePass KDBX vaults through [kpass](https://github.com/Lanjelin/kpass), a pass-like CLI designed for agents.

The instructions in [SKILL.md](SKILL.md) cover finding, reading, creating, and updating entries, generating passwords, TOTP, and vault credential management. This repository contains the skill, not the CLI.

## Install

1. Install kpass 0.2.0 or newer using [pipx](https://pipx.pypa.io/) and Python 3.11 or newer:

   ```sh
   pipx install 'kpass-cli>=0.2.0'
   kpass --version
   ```

   For an existing installation, run `pipx upgrade kpass-cli`. This skill requires kpass 0.2.0 or newer for `--non-interactive`, `generate --quiet`, and the credential-handling fixes.

2. Install the skill in OpenClaw's shared skills directory (default state directory shown):

   ```sh
   mkdir -p ~/.openclaw/skills
   git clone https://github.com/Lanjelin/openclaw-kpass.git ~/.openclaw/skills/kpass
   ```

   For one agent only, clone into `<workspace>/skills/kpass` instead. With a custom `OPENCLAW_STATE_DIR`, use `$OPENCLAW_STATE_DIR/skills/kpass`.

3. Configure access to your existing vault. Replace these example paths:

   ```sh
   export KPASS_DB="/absolute/path/to/vault.kdbx"
   export KPASS_PASSWORD_FILE="/absolute/path/to/vault-password"
   # If your vault also requires a keyfile:
   # export KPASS_KEYFILE="/absolute/path/to/vault.keyx"
   ```

   The password file must contain the vault password on its first line. Restrict its permissions (for example, `chmod 600` on Unix). `KPASS_PASSWORD` is an alternative to the password file; do not put real passwords in shell history, this repository, or chat.

   Make the binary, environment variables, and vault files available to the process that runs agent commands—not just your interactive shell. Sandboxed execution needs access inside the sandbox too. A skill does not grant filesystem or tool access.

4. Start a new OpenClaw session. Check discovery with `openclaw skills info kpass` and try asking: “Use kpass to find entries matching example.com.”

## Safety

Use a dedicated agent vault containing only the credentials the agent needs, and keep backups before allowing writes. `generate` can replace an existing password and prints it unless `--quiet` is used. Default entry views and XML exports can also expose secrets to tool logs; clipboard access requires a working local GUI and is not a headless secret-delivery mechanism.

The skill defaults to targeted searches and field reads, uses `--non-interactive` and `generate --quiet` for unattended execution, and requires authorization for destructive operations. Interactive editing still requires a suitable local session. Review [SKILL.md](SKILL.md) before granting vault access.

Never commit vaults, passwords, password files, or keyfiles. Only authorize deletion or vault credential changes when intended.

## License

[MIT](LICENSE), copyright 2026 Lanjelin. See the upstream kpass repository for its own licensing and documentation.
