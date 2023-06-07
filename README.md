Forcepoint NGFW-SMC Ansible Collection
======================================
[Ansible](https://www.ansible.com) collection that allows us to configure and automate [Forcepoint Next Generation Firewall NGFW](https://www.forcepoint.com/product/network-security/forcepoint-ngfw). It uses [smc-python](https://github.com/Forcepoint/fp-NGFW-SMC-python) api for all operations between ansible client and Forcepoint NGFW Management Center.

#### Prerequisites

* smc-python >= 0.6.0
* Forcepoint NGFW Management Center 7.x
* API client account with permissions
* Python version >= 3.8
* Tested ansible version: 2.9.10

#### Using `virtualenv` (recommended)
```bash
pip install ansible
ansible-galaxy collection install fp_flexedge_secure_sdwan.flexedge_secure_sdwan_ansible
```
* To check if the collection is installed successfully, find it in the list using:
```bash
ansible-galaxy collection list
```
* Enable the SMC API within the management center

#### Usage

Each ansible run will require a login event to the Forcepoint NGFW Management Center to perform it's operations.
Since the ansible libraries use smc-python, the login process uses the same session logic.

* You can provide url and api_key as task parameters
* You can provide the `smc_alt_filepath` parameter in the task run to specify where to find the .smcrc file with your stored credentials

If neither of the two above are used, then:
* Try to find ~.smcrc in users home directory
* Use environment variables (SMC_ADDRESS, SMC_API_KEY, ...)

If none of the above succeed, the run will fail.

Example play:

```yaml
---
- name: Get the network element facts 
  hosts: localhost
  tasks:
    - fp_flexedge_secure_sdwan.flexedge_secure_sdwan_ansible.network_element_facts:
        element: host
        filter: 1.1.1.1
        smc_address: 'http://127.0.0.1:8082'
        smc_api_key: 'vzNkPjuot9JWcRNH4Q5kQ5hC'
        smc_api_version: '7.1'
        smc_timeout: 15
```
[For more information on how to use the collection in playbooks](https://docs.ansible.com/ansible/latest/collections_guide/collections_using_playbooks.html)

#### Running playbooks

Before running plays, it's best to explain the architecture used to make the administrative changes.

The Forcepoint NGFW Management Center is where modifications to all elements are performed.

Installing the ansible modules can therefore either be done on a client host machine remote from the SMC, or on the SMC itself.

If the ansible modules are installed on a controller that is remote from the SMC, set your inventory to use localhost for the connection.

For example, set your default inventory */etc/ansible/hosts*:
```
localhost ansible_connection=local
```
Note that the host running the ansible client will still need to connect to the SMC through the smc-python API over the default port 8082/tcp.

The other option is to install the ansible libraries on the SMC server itself and make your ansible runs from the controller client. In this case, the SMC connection can then be done using an SMC url of 127.0.0.1.

#### More information

All modules provide doc snippets when run from the ansible client:

```
ansible-doc -s engine
```

#### Contributions

If you have requests for additional configuration functionality, please submit an issue request.
 
