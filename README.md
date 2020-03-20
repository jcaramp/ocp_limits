ocp_limits
=========

This role implements tasks for set the Resource Limits defined in an Openshift project.

Requirements
------------

The below requirements are needed on the host that executes this role.

Minimum Ansible version: 2.8.0

Ansible modules:

- k8s
- k8s_auth
- k8s_info / k8s_facts

Python modules:

- python >= 2.7
- openshift >= 0.6
- PyYAML >= 3.11
- urllib3
- requests
- requests-oauthlib

Role Variables
--------------

Roles needed to use this role:

Variable | Description | Required | Choices/***Defaults***
------------ | ------------- | ------------- | -------------
api_url | Openshift API URL | yes | -
ocp_username |  Openshift Username | no | -
ocp_password | Openshift Password | no | -
ocp_token | Openshift Service Account Access Token | no | -
ocp_verify_ssl | Verify SSL | no | ***true***, false
project_name | Openshift Project Name | yes | -
limit_ranges | Openshift Resource Limits definition | yes | **\***

**\*** See file defaults/main.yml to know the default defined **`limit_ranges`** variable ***(it can always be redefined)***.

Dependencies
------------

No dependencies.

Example Playbook
----------------

Using this variable example definition:

    # Limits Range definition
    limits:
      - name: resource-limits
        limit:
          - type: Container
            default:
              cpu: 1
              memory: 1280Mi
            defaultRequest:
              cpu: 100m
              memory: 512Mi

This is an example using an API token to authenticate:

    - hosts: servers
      roles:
        - role: ocp_limits
          vars:
            api_url: "https://openshift.example.com:6443"
            ocp_token: "{{ service_account_token }}"
            ocp_verify_ssl: false
            project_name: "example-project"
            limit_ranges: "{{ limits }}"

And this is an example using an user/password to authenticate:

    - hosts: servers
      roles:
        - role: ocp_limits
          vars:
            api_url: "https://openshift.example.com:6443"
            ocp_username: "clusteradmin"
            ocp_password: "xxxxxxxxxxx"
            ocp_verify_ssl: true
            project_name: "example-project"
            limit_ranges: "{{ limits }}"

Platforms
------------

Tested on:

- Red Hat Enterprise Linux 7.7
- Red Hat Openshift Container Platform 4.2

License
-------

GNU General Public License v3.0

Author Information
------------------

This role was written in 2020 by Jesús Carmona Ampuero
