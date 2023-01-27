# Blocklisterd

Install and configure [Blocklister](https://github.com/dlford/blocklister), a daemon written in Go for processing IP block list (TXT) files into iptables rules and keeping them updated regularly.

## Requirements

Ubuntu

## Role Variables

- `blocklisterd_config`: [Blocklister config object](https://github.com/dlford/blocklister/tree/v2#configuration)
- `blocklisterd_major_version`: (optional) Major release of Blocklister for updates, e.g. `v2`, [see all releases](https://github.com/dlford/blocklister/releases), defaults to `v2`
- `blocklisterd_start_after`: (optional) Applies to systemd service file, defaults to `[ "network-online.target" ]`, you should always include `network-online.target` if this value is changed

## Dependencies

- `systemd`

## Example Playbook

```yml
- hosts: servers
  roles:
      - role: dlford.blocklisterd
        vars:
          # Update Blocklister within this major version when playbook is run
          blocklisterd_major_version: v2
          # Systemd service file `After=` args
          blocklisterd_start_after:
            - network-online.target
            - docker.service # Added Docker service, because it creates the `DOCKER-USER` chain
          blocklisterd_config:
            # Cron syntax, default list updating schedule for all lists
            schedule: "*/15 * * * *" # Every 15 minutes
            # Blocklists, add as many as needed
            lists:
              # Title will be used for `ipset` name
              - title: ipsum
                # URL to a TXT file with a list of IP addresses to block
                url: https://raw.githubusercontent.com/stamparm/ipsum/mastejr/ipsum.txt
                # iptables chains to block IPs from, add as many as needed
                chains:
                  # Default inbound traffic chain is INPUT
                  - INPUT
                  # Docker published ports skip the INPUT chain,
                  # the DOCKER-USER chain is for user rules
                  - DOCKER-USER
                # [optional] max number of elements in set, default is 65536, increase for larger lists
                max_elem: 300000
                # [optional] Cron syntax, overrides default schedule
                schedule: "0 0 * * *" # Every day at midnight
```

## License

MIT

## Author Information

https://www.dlford.io
