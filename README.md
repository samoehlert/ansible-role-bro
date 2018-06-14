# Ansible Role: Bro IDS

Install Bro IDS

## Requirements

Doesn't require any other roles.

This module will likely only work on RHEL/Centos 7 based hosts. We host our myri_snf rpm in a repository we run. I would suggest the same.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`). These default settings would set up a cluster in a box type of set up:

Basic set up vars

	bro_user: "bro"
	bro_home: "/home/{{ bro_user }}"
	bro_branch: 'v2.5.4'

Which interface to make some ethtool tweaks to

	bro_ethtool_iface: eth0

Directory to download source code to and where to install to

	bro_build_path: "/usr/local/src/bro_{{ bro_branch }}"
	bro_path: "/usr/local/bro"

Build commands for bro

	bro_build_commands:
		- ./configure --prefix="{{ bro_path }}"
		- make
		- make -j install

Info for broctl.cfg

	bro_mailto_user: "bro@localhost"
	bro_log_rotation: "3600"
	bro_log_expire: "30"
	bro_disk_space: "5"

Info for networks.cfg template

	bro_networks:
		- 192.168.0.0/16
		- 172.16.0.0/12
		- 10.0.0.0/8

Info for node.cfg:
This part sets up the manager node

	bro_manager:
		node_name: 'localhost'
		node_ip: '127.0.0.1'

Optional (for use with myricom)

		myricom_ring_size: '16384'

This part sets up the worker node(s)

	bro_worker:
		- node_name: 'localhost'
		  node_ip: '127.0.0.1'
		  bro_interface: 'eth0'

Optional (we use with myricom)

		  lb_method: 'custom'
		  lb_procs: '14'
		  pin_cpus: '2,3,4,5,6,7,8,9,10,11,12,13,14,15'
		  bro_env_vars: 'SNF_FLAGS=0x1'

If you want to use myricom, set this to true

	myricom: false

## Dependencies

None.

## Example Playbook

Example using defaults to get a cluster in a box setup

    - hosts: bro
      roles:
        - samoehlert.bro

## License

MIT / BSD

## Author Information

This role was created in 2017 by [Sam Oehlert at ESnet](https://es.net/)
