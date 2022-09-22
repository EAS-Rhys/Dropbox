<img src="http://localhost:8080/banner-slim.png" alt="drawing" height="135px"/>
 
 `Technology and Release status`
 
<img src="http://localhost:8080/ansible-badge.png" alt="drawing" height="29px"/> <img src="http://localhost:8080/status-in development.png" alt="drawing" height="28px"/> 

`Supported platforms`

<img src="http://localhost:8080/platforms-windows-rhel.png" alt="drawing" height="28px"/> 

## Introduction

The guardrails capability Ansible role is a utility role that facilitates the pre and post checks required by an application deployment. It ensures files, folders and operating system level firewall rules are in the correct state at the required points of the deployment sequence.

## Usage

All capability roles are consumed as part of the Application Automation GitOps pipeline process guided by the contents of the manifest.yaml file in the application source repository as detailed <here>.


## Features

- File/Folder Checksum
	- Pre change retrieval
	- Post change retrieval and comparison
- File/Folder Security
	- Pre change retrieval
	- Post change retrieval and comparison
- Firewall Ports
	- Pre change rules retrieval
	- Post change rules retrieval and comparison

## Consumption

Features in this capability can be consumed in the Application Automation process using the manifest.yaml file. Include this capability in the manifest using the following syntax.

```
components:
	- capability_name: guardrails
	  capability_version: STRING <required tag version>
	  sequence_id: NUMBER <order to execute the capability feature in>
	  capability_groups: []STRING [<inventory_group>]
	  capability_params: OBJECT <options detailed below>
```

The capability can be included multiple times to use different capability features at different stages of the application automation manifest.

### Feature Syntax

##### File/Folder Checksum

- *Pre*
```
capability_params:
	guardrails_checksum_pre_files:
		- name: STRING <friendly identifier - no spaces or special characters>
		  path: STRING <path to file/folder>
```
- *Post*
```
capability_params:
	guardrails_checksum_post_files:
		- name: STRING <must match a pre file for comparison to happen>
		  changed: BOOLEAN <File should have changed or not>
```

##### File/Folder Security

- *Pre*
```
capability_params:
	guardrails_security_pre_linux_files:
		- name: STRING <friendly identifier - no spaces or special characters>
		  path: STRING <path to file/folder>
```
- *Post*

>At least one of [changed, mode, owner, group] must be included for a comparison to take place.


```
capability_params:
	guardrails_security_post_linux_files:
		- name: STRING <must match a pre file for comparison to happen, compares security attrs>
		  changed: BOOLEAN - OPTIONAL <File should have changed or not> 
		  mode: NUMBER - OPTIONAL <desired file permissions in octal notation>
		  owner: STRING - OPTIONAL <desired owner user>
		  group: STRING - OPTIONAL <desired owner group>
```

##### Firewall

- *Pre*
```
capability_params:
	guardrails_firewall_pre_rules:
		- name: STRING <friendly identifier - no spaces or special characters>
```
- *Post*
>At least one of [changed, changes] must be included for a comparison to take place.
```
capability_params:
	guardrails_firewall_post_rules:
		- name: STRING <must match a pre firewall for comparison to happen>
		  changed: BOOLEAN - OPTIONAL <Complete ruleset comparison, changes allowed or not> 
		  changes: []OBJECT - OPTIONAL 
			  - name: STRING <friendly identifier - no spaces or special characters>
			    port: NUMBER <port number to compare>
			    protocol: STRING <protocol UDP/TCP>
			    state: STRING <state port should be in: open/closed>
```

Full syntax JSONSchema validation file can be found < HERE > 
