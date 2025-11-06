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


## Usage

You will need to install the `ansible.posix` collection if you don't have it already:

```bash
 ansible-galaxy collection install ansible.posix
```

Run the playbook with the following command, replacing the variables with your own values:

```bash
ansible-playbook node.yml -i hosts.yml
```

## TODO 

- [ ] cron job to prune old data 
- [ ] nginx service to serve info endpoint
- [ ] monitoring with prometheus

## License

MIT



