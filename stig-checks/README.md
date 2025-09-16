
# Ubuntu STIG Manual Check – Ansible Bundle

## Files
- `inventory.ini` — put your target hosts here.
- `commands.json` — list of RuleID/Command/Notes used by the playbook (as a `commands` var).
- `stig_checks.yml` — main playbook; runs all commands with `become: true` and collects results.
- `commands_clean.csv` — cleaned CSV (no blank commands).
- Results will be written to `./results/stig_results_<host>_<timestamp>.json` on your Ansible controller, and `/tmp/stig_results.json` on each target.

## Requirements
- Ansible on your controller machine.
- Python `jmespath` library on the controller (for `json_query`): `pip install jmespath`
- SSH access to targets; user with sudo privileges.

## Usage
1. Edit `inventory.ini` with your hosts (or use your existing inventory).
2. Run:

```bash
ansible-playbook -i inventory.ini stig_checks.yml \
  -u <ssh_user> --key-file <path_to_key> \  -e ansible_become=yes
```

If sudo requires a password, add `--ask-become-pass`.

## Notes
- Commands that already include `sudo` will still work; `become: true` is set globally.
- Empty "Command" entries from your CSV are skipped in the cleaned files.
- Shell uses `/bin/bash` and `set -o pipefail` so pipelines fail correctly.
