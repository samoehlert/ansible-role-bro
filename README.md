# Ansible Role: Bro IDS

Install Bro IDS

## Requirements

Doesn't require any other roles.

This module will likely only work on RHEL/Centos 7 based hosts.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

	bro_user: "bro"
	bro_mailto_user: "bro@localhost"
	bro_home: "/home/{{ bro_user }}"
	bro_branch: 'v2.5.4'
	bro_ethtool_iface: eth0
	bro_build_path: "/usr/local/src/bro_{{ bro_branch }}"
	bro_path: "/usr/local/bro"
	bro_log_rotation: "3600"
	bro_log_expire: "30"
	bro_disk_space: "5"
	bro_build_commands:
		- ./configure --prefix="{{ bro_path }}"
		- make
		- make -j install
	bro_networks:
		- 192.168.0.0/16
		- 172.16.0.0/12
		- 10.0.0.0/8
	bro_manager:
		node_name: 'localhost'
		node_ip: '10.0.0.1'
		##### Optional (for use with myricom)
		myricom_ring_size: '16384'
	bro_worker:
		- node_name: 'localhost'
		  node_ip: '127.0.0.1'
		  bro_interface: 'eth0'
		  ##### Optional (we use with myricom)
		  lb_method: 'custom'
		  lb_procs: '14'
		  pin_cpus: '2,3,4,5,6,7,8,9,10,11,12,13,14,15'
		  bro_env_vars: 'SNF_FLAGS=0x1'
	myricom: false
	myricom_build_commands:
		- ./configure --with-myricom=/opt/snf
		- make -j
		- make -j install

## Dependencies

None.

## Example Playbook

    - hosts: servers
      vars:
        backup_identifier: "{{ inventory_hostname|replace('.', '') }}"
        backup_user: "backupuser"
        backup_remote_connection: user@backups.example.com
        backup_hour: "1"
        backup_minute: "15"
        backup_mysql: false
        backup_directories:
          - /etc/myapp
          - /var/myapp/data
          - /home/myuser
      roles:
        - geerlingguy.backup

## License

MIT / BSD

## Author Information

This role was created in 2017 by [Sam Oehlert at ESnet](https://es.net/)
