# UseGalaxy ansible_ssh_manager

This Ansible role provides a centralized way to manage SSH authorized keys for multiple users across multiple machines. It allows you to define SSH keys for different people and specify which machines and user accounts they should have access to.

## Requirements

Ansible >= 2.11

## Installation

Install the collection via ansible-galaxy:

`ansible-galaxy install usegalaxy_eu.ssh_manager`

## Role Variables
| Vaiable | Descriptions |
| ------- | ------------ |
| `ssh_manager_authorized_persons`  | A dictionary defining which persons have SSH access to which machines and user accounts. |

**Structure:**
```yaml
ssh_manager_authorized_persons:
  <person_name>:
    keys:
      - '<ssh_public_key_1>'
      - '<ssh_public_key_2>'
    machines:
      - <hostname_or_pattern>
      - all  # special keyword for all machines
    users:
      - <unix_username_1>
      - <unix_username_2>
```

## How It Works

1. The role retrieves the user database from each target machine
2. It collects all UNIX users that should have SSH keys configured based on:
   - Users listed in `ssh_manager_authorized_persons`
   - Users that already have an `authorized_keys` file
3. For each user, it builds a list of SSH keys by:
   - Matching the current machine against the `machines` pattern
   - Matching the UNIX user against the `users` list
   - Collecting all matching keys
4. The role then updates the `authorized_keys` file for each user with the appropriate keys

**Note:** This role will **overwrite** the `authorized_keys` file. Any keys not defined in `ssh_manager_authorized_persons` will be removed.

## License

MIT

## Author Information

This role was created by the [UseGalaxy.eu](https://usegalaxy.eu) team.
