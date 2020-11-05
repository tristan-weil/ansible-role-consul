# Ansible Role: consul

An Ansible Role that installs and configures `consul`.

**NOTE**: this role uses the integrated package system for the folowing OS:
- OpenBSD

[![Actions Status](https://github.com/tristan-weil/ansible-role-consul/workflows/molecule/badge.svg?branch=master)](https://github.com/tristan-weil/ansible-role-consul/actions)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

Mandatory variables:

| Variable      | Description |
| :------------ | :---------- |
| consul_config | a dictionnary of parameters to configure consul 

Optional variables:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| consul_certs_dir | /etc/ssl/consul | the path where SSL certs are stored |
| consul_config_tls_files | [] | a list of certs to store |

If the package system is not used, the following variables are available to handle directly the artifacts:

| Variable      | Default | Description |
| :------------ | :------ | :---------- |
| consul_artifact_version | 1.7.2 | the version |
| consul_artifact_url | https://releases.hashicorp.com/consul/....zip | the path to the artifacts to install |
| consul_artifact_sum_url  | https://releases.hashicorp.com/consul/..._SHA256SUMS | the path to the SUMS file |
| consul_artifact_signature_url | https://releases.hashicorp.com/consul/..._SHA256SUMS.sig | the path to the SUMs signatures file| 
| consul_artifact_sum_algo | sha256 | the hash algorithm used in `consul_artifact_sum_url` |
| consul_pgp_key | ... | the PGP key of the project |
| consul_artifact_version_flag_path | /etc/consul.version | the files prevents reinstallation of the artifacts |
| consul_artifact_tmpdir | /tmp/consul | the installation work directory |
| consul_force_update | False | *True/False* to force the update of the artifacts |
| consul_daemon_nofiles | 10000 | the maximum number of files the daemon can open |
| consul_daemon_nproc | 512 | the maximum number of processes the daemon can have |

## Example Playbook

    - hosts: 'webservers'
      tasks:
        - import_role: 
            name: 'ansible-role-consul'
          vars:
            consul_config:
              enable_syslog: True
              datacenter: 'bi-sbg'
              domain: 'consul.'
              node_name: "{{ ansible_facts['hostname'] }}"
              leave_on_terminate: True
              skip_leave_on_interrupt: True
              bind_addr: "{{ '{{' }} GetInterfaceIP `eth0` {{ '}}' }}"
              client_addr: '127.0.0.1'
              retry_join:
                - 192.168.0.10
                - 192.168.0.11
                - 192.168.0.12
              disable_host_node_id: True
              encrypt: "xxxxxxx"
              encrypt_verify_incoming: True
              encrypt_verify_outgoing: True
              ui: True
              enable_script_checks: False
              enable_local_script_checks: True
              disable_remote_exec: True
              disable_update_check: True
              verify_incoming: False
              verify_outgoing: True
              verify_server_hostname: True
              ca_path: "{{ consul_certs_dir }}/consul-ca.pem"
              auto_encrypt:
                tls: True
              log_level: 'INFO'
              telemetry:
                prometheus_retention_time: '5m'
                disable_hostname: True

## Todo

None.

## Dependencies

See [requirements_galaxy.yml](https://github.com/tristan-weil/ansible-role-consul/blob/master/requirements_galaxy.yml)

## Supported platforms

See [meta/main.yml](https://github.com/tristan-weil/ansible-role-consul/blob/master/meta/main.yml)

## License

See [LICENSE.md](LICENSE.md)
