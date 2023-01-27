# Blocklisterd

Install and configure [Blocklister](https://github.com/dlford/blocklister), a daemon written in Go for processing IP block list (TXT) files into iptables rules and keeping them updated regularly.

## Requirements

Ubuntu

## Role Variables

- `config`: [Blocklister config object](https://github.com/dlford/blocklister/tree/v2#configuration)
- `major_version`: (optional) Major release of Blocklister for updates, e.g. `v2`, [see all releases](https://github.com/dlford/blocklister/releases), defaults to `v2`
- `start_after`: (optional) Applies to systemd service file, defaults to `[ "network-online.target" ]`, you should always include `network-online.target` if this value is changed

## Dependencies

- `systemd`

## Example Playbook

    - hosts: servers
      roles:
         - role: dlford.blocklister
           vars:
             major_version: v2
             start_after:
               - network-online.target
               - docker.service
             config:
               schedule: "*/15 * * * *"
               lists:
                 - title: ipsum
                   url: https://raw.githubusercontent.com/stamparm/ipsum/master/ipsum.txt
                   chains:
                     - INPUT
                     - DOCKER-USER

## License

MIT

## Author Information

https://www.dlford.io
