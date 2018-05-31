# Exposeur role

Ufw management with iptables fixes for control docker exposing 

## Usage

By default role allow only `ssh` access

Add to ansible playbook following:

    - import_role:
        name: dr.docker
      vars:
        # reset ufw rules before other actions
        expo_reset_ufw: no
        # do setup ufw
        expo_setup_ufw: yes
        # use limit instead if allow
        expo_limit_ssh: yes
        # Docker interface that will be used
        expo_docker_interface: docker0
        # Use this rules when you expose docker container to 0.0.0.0 but need to limit access.
        # Enabling blocks all incoming traffic by default. This rules allows certain directions
        expo_dockiptfix_apply: no
        expo_dockiptfix_rules: []
        #  - { from_ip: '{{rodin_vpn}}'}
        #  - { from_ip: '{{office_ip}}'}
        # Regular ufw rules
        expo_rules: []
        # Use this list to define exposing services that listed on ingernal interface.
        # It generates DNAT rules for indicaged directions
        expo_expose_rules: []
        # example default values: chain: 'nat|PREROUTING' ;  in_i: '{{expo_main_int}}'; rule: 'dnat'
        #  - { chain: 'nat|PREROUTING', in_i: '{{expo_main_int}}', from_ip: '{{rodin_vpn}}', to_ip: '{{expo_main_ip}}', to_port: 25, rule: 'dnat', to_dest: '172.16.25.1' }
        # By default exposer handles only main system interface, you can add additinal by passing ip adds
        expo_exposed_ips: ['123.23.23.11']
        # Set ufw policies
        expo_policies:
        - { direction: 'incoming', policy: 'deny' }
        - { direction: 'outgoing', policy: 'allow' }
        - { direction: 'routed', policy: 'deny' }
      tags: ['firewall']
      become: yes

if you need more flexibility use next params:

        expo_main_int: '{{ansible_default_ipv4.alias}}'
        expo_main_ip: '{{ansible_default_ipv4.address}}'
        expo_service_name: dr-iptables
        expo_config_dir: /etc/dr-iptables


## Default parameters

Discover in `defaults/main.yml`
