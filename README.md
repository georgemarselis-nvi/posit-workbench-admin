# posit-workbench-admin

Bash completion and session management wrapper for Posit Workbench (`rstudio-server`).

## Features

- Tab completion for `rstudio-server` subcommands
- Live session token completion for `kill-session` and `suspend-session` in `username_DisplayName:sessionId` format
- Live username completion for user management subcommands
- `rstudio_active_sessions` — lists active sessions with display names resolved from Active Directory via LDAP/Kerberos, session duration in days/hours/minutes
- `rstudio-server` wrapper that strips `username_DisplayName:sessionId` tokens to bare sessionId for kill/suspend subcommands

## Requirements

- Posit Workbench (tested on 2025.09.0 on RHEL 9)
- `jq`
- `mlr` (miller)
- `openldap-clients` (`ldapsearch`)
- Kerberos (`klist`, `kinit`)

## Installation

1. Copy `rstudio_server` to `/etc/profile.d/rstudio_server` on your Workbench host
2. Create `/etc/profile.d/rstudio_server.conf` (not committed — keep out of version control):

```bash
RSTUDIO_SERVER_BIN="/usr/sbin/rstudio-server"
LDAP_HOST="ldap://dc.example.com:3268"
LDAP_BASE="dc=example,dc=com"
LDAP_DOMAIN="example.com"
```

3. Source it or log in again:

```bash
source /etc/profile.d/rstudio_server
```

## Usage

```bash
# list active sessions with display names and duration
rstudio_active_sessions

# tab complete session tokens for kill/suspend
rstudio-server kill-session <TAB>

# tab complete usernames for user management
rstudio-server lock-user <TAB>
```

## Notes

Some subcommands are confirmed as non-functional on a standard single-node Workbench installation without load balancing or launcher configured:

- `suspend-session`, `suspend-all`, `force-suspend-session`, `force-suspend`
- `list-nodes`, `node-status`, `delete-node`, `reset-cluster` (load balancing disabled)
- `configure-vs-code`, `install-vs-code-ext`, `install-positron-ext`

## License

GPL-3.0
