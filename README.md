prepare-splunk
=========

Prepares fresh VMs/cloud instances to deploy a distributed multisite Splunk cluster with splunk-ansible official playbook.

* Tested with CentOS 7/8
* Runs under root

Example Playbook
----------------
Including an example of how to use this role (for instance, with variables passed in as parameters, which [can be evaluated here](https://github.com/splunk/splunk-ansible/blob/develop/docs/ADVANCED.md)):

    - hosts: splunk-servers
      gather_facts: yes
      strategy: free
      vars:
        version: 8.2.1
        build: ddff1c41e5cf # This is the build number for the version you want to install, for instance, https://github.com/splunk/docker-splunk/blob/develop/Makefile#L11
        license_uri: https://1.2.3.4:8089
        password: your_password
        shc_pass4symmkey: your_shc_pass4symmkey
        shc_label: your_shc_label
        idxc_pass4symmkey: your_idxc_pass4symmkey
        idxc_discoverypass4symmkey: your_idxc_pass4symmkey
        idxc_label: your_idxc_label
        pass4symmkey: your_shc_pass4symmkey
        multisite_replication_factor_origin: 1
        multisite_replication_factor_total: 3
        multisite_search_factor_origin: 1
        multisite_search_factor_total: 3
        all_sites:
        - site1
        - site2
        ...
        - siteN
        - siteN+1
        # Check other variables from here https://github.com/splunk/splunk-ansible/blob/develop/docs/ADVANCED.md
      roles:
        - prepare
    post_tasks:
      - name: Touch ansible log file
        ansible.builtin.file:
          path: /opt/container_artifact/ansible.log
          state: touch
          owner: ansible
          group: ansible

      - name: Execute splunk-ansible deployment
        shell: cd /opt/ansible/ && ansible-playbook -i inventory/environ.py --connection local multisite.yml

License
-------

BSD

Author Information
------------------

Bulat Gumerov
