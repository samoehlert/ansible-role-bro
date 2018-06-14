# Ansible Role: Bro IDS

Install Bro IDS in either cluster in a box or a multi node cluster set up.

## Requirements

Doesn't require any other roles.

This module will likely only work on RHEL/Centos 7 based hosts. We host our myri_snf rpm in a repository we run. I would reccomend the same.

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

Example using extra variables to set up a multinode cluster using myricom nic

	- hosts: bro
	  vars:
		bro_user: "bro"
		bro_home: "/home/{{ bro_user }}"
		bro_branch: 'v2.5.4'
		bro_ethtool_iface: eth0
		bro_build_path: "/usr/local/src/bro_{{ bro_branch }}"
		bro_path: "/usr/local/bro"
		bro_build_commands:
			- ./configure --prefix="{{ bro_path }}"
			- make
			- make -j install
		bro_mailto_user: "bro@localhost"
		bro_log_rotation: "3600"
		bro_log_expire: "30"
		bro_disk_space: "5"
		bro_networks:
			- 192.168.0.0/16
			- 172.16.0.0/12
			- 10.0.0.0/8
		bro_manager:
			node_name: 'bro1'
			node_ip: '10.0.0.1'
			myricom_ring_size: '16384'
		bro_worker:
			- node_name: 'bro2'
			  node_ip: '10.0.0.2'
			  bro_interface: 'eth0'
			  lb_method: 'custom'
			  lb_procs: '14'
			  pin_cpus: '2,3,4,5,6,7,8,9,10,11,12,13,14,15'
			  bro_env_vars: 'SNF_FLAGS=0x1'
			- node_name: 'bro3'
			  node_ip: '10.0.0.3'
			  bro_interface: 'eth0'
			  lb_method: 'custom'
			  lb_procs: '14'
			  pin_cpus: '2,3,4,5,6,7,8,9,10,11,12,13,14,15'
			  bro_env_vars: 'SNF_FLAGS=0x1'
		myricom: true
      roles:
        - samoehlert.bro

## License

ansible-role-bro, Copyright (c) 2014-2018, The Regents of the University of California, through Lawrence Berkeley National Laboratory (subject to receipt of any required approvals from the U.S. Dept. of Energy). All rights reserved.

If you have questions about your rights to use or distribute this software, please contact Berkeley Lab's Technology Transfer Department at TTD@lbl.gov.

NOTICE. This software is owned by the U.S. Department of Energy. As such, the U.S. Government has been granted for itself and others acting on its behalf a paid-up, nonexclusive, irrevocable, worldwide license in the Software to reproduce, prepare derivative works, and perform publicly and display publicly. Beginning five (5) years after the date permission to assert copyright is obtained from the U.S. Department of Energy, and subject to any subsequent five (5) year renewals, the U.S. Government is granted for itself and others acting on its behalf a paid-up, nonexclusive, irrevocable, worldwide license in the Software to reproduce, prepare derivative works, distribute copies to the public, perform publicly and display publicly, and to permit others to do so.

This code is distributed under a BSD style license.

## Author Information

This role was created in 2017 by [Sam Oehlert at ESnet](https://es.net/)
