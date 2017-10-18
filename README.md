Role Name
=========

The te_axon role installs, configures, and manages the services of the
Tripwire Axon Agent.

Requirements
------------

The Tripwire Axon Agent needs a Tripwire Enterprise Console server
to connect to. A DNS SRV record can be created so that the Agent can discover
the Console server automatically. Otherwise, the Axon bridge host and port must be provided.
If the Axon bridge requires a key, the key must be provided as a parameter as well.

The installer file(s) for the Agent must also be available on the Ansible
control machine, for copying to the remote host for installation.

Role Variables
--------------

```yaml
# REQUIRED variables (no default defined)
#########################################

# Must be set to the path to the agent installer. Is copied to a
# temporary directory on the remote host for installation
te_axon_package_source: ~

# Must be set to the path to the event generator service RPM.
# Not used/required on Windows.
te_axon_package_eg_source: ~
# Must be set to the path to the event generator driver RPM.
# If set to a driver other than DKMS, te_axon_package_eg_driver_name must also be set.
# Not used/required on Windows.
te_axon_package_eg_driver_source: ~

# OPTIONAL variables (no default)
#################################

# If set, is used as the pre-shared registration key for the agent
te_axon_registration_key: ~
# If set, is used as the hostname or IP of the Axon bridge to connect to
# instead of resolving by DNS SRV.
te_axon_bridge_host: ~
# If set, used to determine if the package needs to be upgraded
# On Windows, this must be the 4-part DisplayVersion (e.g. 3.6.0.1689)
te_axon_package_version: ~
# If set, is written to the agent tags file for initial registration
te_axon_tags: ~

# from defaults/main.yml
te_axon_package_state: present
te_axon_package_install_path: '/opt/tripwire'
te_axon_package_eg_state: present
te_axon_package_eg_driver_name: tw-eg-driver-dkms
te_axon_dns_srvc_name: '_tw-agw'
te_axon_dns_srvc_domain: ~
te_axon_bridge_host: ~
te_axon_bridge_port: 5670
te_axon_registration_filename: 'registration_pre_shared_key.txt'
te_axon_proxy_hostname: ~
te_axon_proxy_port: ~
te_axon_proxy_username: ~
te_axon_proxy_password: ~
te_axon_tls_version: ~
te_axon_tls_cipher_suites: ~
te_axon_logger_level: ~
te_axon_spool_size: '1g'
te_axon_service_state: started
te_axon_service_enabled: true
te_axon_service_eg_state: started
te_axon_service_eg_enabled: true

# from vars/defaults.yml (should not be changed)
te_axon_config_path: '/etc/tripwire'
te_axon_package_name: axon-agent
te_axon_package_eg_name: tw-eg-service
te_axon_service_name: tripwire-axon-agent
te_axon_service_eg_name: tw-eg-service

# from vars/Windows.yml (should not be changed)
te_axon_config_path: 'C:\ProgramData\Tripwire\Agent\config'
te_axon_package_name: '{6AE2368F-20CD-42B3-B355-5092C00D70E2}'
te_axon_service_name: TripwireAxonAgent
te_axon_service_eg_name: TripwireEventGeneratorService
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - role: te_axon
      te_axon_package_source: /mnt/data/te_axon/linux/x86_64/axon-agent-installer-linux-x64.rpm
      te_axon_package_eg_source: /mnt/data/te_axon/linux/x86_64/tw-eg-service-1.3.326-1.x86_64.rpm
      te_axon_package_eg_driver_source: /mnt/data/te_axon/linux/x86_64/tw-eg-driver-rhel-1.3.313-1.x86_64.rpm
      te_axon_package_eg_driver_name: tw-eg-driver-rhel
      te_axon_registration_key: correct horse battery staple
      te_axon_tags:
        taga: [tag1]
        tagb: [tag2, tag3]
      te_axon_package_version: 3.6.0
```

 License
 -------

 Licensed under the Apache 2.0 license. See the LICENSE and NOTICE files for details.

 Author Information
 ------------------

 Copyright 2017 Tripwire, Inc.

 https://www.tripwire.com/
