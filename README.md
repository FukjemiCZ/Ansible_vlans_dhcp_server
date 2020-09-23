# Ansible_vlans_dhcp_server
Ansible Dhcp server for vlans
Example:
subnet 192.168.0.0/24 Disable DHCP server vlan untaged
subnet 192.168.1.0/24 Enabled DHCP server tagged 100
subnet 192.168.2.0/24 Enabled DHCP server tagged 200

- name: "Install and Confugarition DHCP_server_"
  hosts: DHCP_servers
  become: true
  roles:
    - fukjemicz.isc_dhcp_server
  vars:
    vlans:
      - interface: eth0
        vlan: untaged
        dhcp_server_address: 192.168.0.2/24
        routers: 192.168.0.1
        dns:
          - 8.8.8.8
          - 1.1.1.1
    subnet:
      - network: 192.168.1.0
        parrent_interface: eth0
        netmask: 255.255.255.0
        broadcast: 192.168.1.255
        routers: 192.168.1.1
        dhcp_server_address: 192.168.1.2/24
        vlan: 100
        domain: "hosts.local"
        dns:
          - 8.8.8.8
          - 1.1.1.1
        range:
          - "192.168.1.100 192.168.1.150"
        get_lease_hostnames: "true"
        use_host_decl_names: "true"
        default_lease_time: 600
        max_lease_time: 7200
      - network: 192.168.2.0
        netmask: 255.255.255.0
        broadcast: 192.168.2.255
        routers: 192.168.2.1
        dhcp_server_address: 192.168.2.2/24
        vlan: 200
        domain: "dmz.local"
        dns:
          - 8.8.8.8
          - 1.1.1.1
        range:
          - "192.168.2.5 192.168.2.130"
        get_lease_hostnames: "true"
        use_host_decl_names: "true"
        default_lease_time: 600
        max_lease_time: 7200
