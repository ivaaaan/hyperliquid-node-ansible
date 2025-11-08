# Hyperliquid non-valiidator node playbook

Ansible playbook to setup a non-validator Hyperliquid node.


## Requirements

- For machine specs refer to [Hyperliquid docs](https://github.com/hyperliquid-dex/node?tab=readme-ov-file#machine-specs).
- Ansible installed on your local machine  


## Configuration

Configuration variables are defined in the `vars.yml` file. Feel free to override them as needed. 

### `gossip_config`

`gossip_config` variable defines the gossip configuration for the Hyperliquid node. You can provide a custom list of IP addresses or use `download_peers` which will get a list of peers using this command: 

```bash
curl -X POST --header "Content-Type: application/json" --data '{ "type": "gossipRootIps" }' https://api.hyperliquid.xyz/info
```

### `visor`

Visor configures a systemd service that will run the node. You can customize the command to write data that you need.

## Roles

To run a specific role specify its playbook: 

```
ansible-playbook playbooks/<role>.yml -i hosts.yml
```

### `base`

Base role installs necessary packages and dependencies for the Hyperliquid node. It will also disable IPv6 on the system and open 4001 and 4002 ports in the firewall which are required for the node to function properly.


### `users`

Users role creates a dedicated user and group for running the Hyperliquid node.

### `node`

Node role downloads and configures the Hyperliquid node binary. It sets up the necessary directories and configuration files based on the provided variables, and starts systemd service to run the node.

### `pruner`

Pruner role sets up a cron job to periodically prune old data from the Hyperliquid node to save disk space. The pruning frequency and retention period can be configured via variables.


### `observer`

Observer role deploys Grafana, Prometheus, and Node Exporter using Docker to monitor the server health. Grafana port is exposed on 3000, so if you need additional security consider setting up a reverse proxy with authentication.

## Usage

You will need to install the `ansible.posix` collection if you don't have it already:

```bash
ansible-galaxy collection install ansible.posix
ansible-galaxy collection install community.crypto
ansible-galaxy collection install geerlingguy.docker
```

Run the playbook with the following command, replacing the variables with your own values:

```bash
ansible-playbook playbooks/hl-node.yml -i hosts.yml
```

### Running specific role



## TODO 

- [x] cron job to prune old data 
- [ ] nginx service to serve info endpoint
- [ ] monitoring with prometheus

## License

MIT



