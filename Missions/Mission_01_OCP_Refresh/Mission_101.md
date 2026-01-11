# Mission 1: OCP Refresh
    **Date:** 11/01/2026
    **Time:** 12:45 - 14:00

---
## Objective:
    Build a small and secure network that uses VLANs and basic security protocols.

## Technical requirements:
    **Program used:** Cisco Packet tracer
    **Topology:** 1 router (router 2901) - 1 switch (switch 2960) - 3 PCs (Admin_PC ; Stuff_PC ; Guest_PC)
    **Segmentation:** 3 VLANs (Vlan 10 [Admin] ; Vlan 20 [Stuff] ; Vlan 30 [Guest])

---

## Deployement steps:

### Prepare the topology:
    **time:** 5 min
    **Network:** 192.168.0.0/24
    **Connections:**
        - Router <> Switch (Straight-Through Cable)
        - Admin_PC <> Switch (Straight-Through Cable)
        - Stuff_PC <> Switch (Straight-Through Cable)
        - Guest_PC <> Switch (Straight-Through Cable)

###     Router configuration:
    **Time:** 15min
    **Configuration:**
        - Secure basics:
        ```    
            enable
            configure terminal
            hostname R1
            no ip domain-lookup
        ```

        - Subinterfaces (VLANs) configuration:
        ```
            interface GigabitEthernet0/0
                no shutdown

            interface GigabitEthernet0/0.10
                encapsulation dot1Q 10
                ip address 192.168.10.1 255.255.255.0

            interface GigabitEthernet0/0.20
                encapsulation dot1Q 20
                ip address 192.168.20.1 255.255.255.0

            interface GigabitEthernet0/0.30
                encapsulation dot1Q 30
                ip address 192.168.30.1 255.255.255.0
        ```

        - DHCP configuration:
        ```
            ip dhcp excluded-address 192.168.10.1 192.168.10.20
            ip dhcp excluded-address 192.168.20.1 192.168.20.20
            ip dhcp excluded-address 192.168.30.1 192.168.30.20

            ip dhcp pool VLAN10
                network 192.168.10.0 255.255.255.0
                default-router 192.168.10.1
                dns-server 8.8.8.8

            ip dhcp pool VLAN20
                network 192.168.20.0 255.255.255.0
                default-router 192.168.20.1
                dns-server 8.8.8.8

            ip dhcp pool VLAN30
                network 192.168.30.0 255.255.255.0
                default-router 192.168.30.1
                dns-server 8.8.8.8

            end
            write memory
        ```

### Switch configuration:
    **Time:** 15 min
    **Configuration:**
        - Secure basics:
        ```
            enable
            configure terminal
            hostname SW1
            no ip domain-lookup
        ```

        - Create VLANs:
        ```
            vlan 10
                name ADMIN
            vlan 20
                name STAFF
            vlan 30
                name GUEST
        ```

        - Assign access ports to VLANs:
        ```
            interface FastEthernet0/1
                switchport mode access
                switchport access vlan 10

            interface FastEthernet0/2
                switchport mode access
                switchport access vlan 20

            interface FastEthernet0/3
                switchport mode access
                switchport access vlan 30
        ```

        - Configure trunk to the router:
        ```
            interface GigabitEthernet0/1
            switchport mode trunk
            switchport trunk allowed vlan 10,20,30
            spanning-tree portfast trunk
            no shutdown
        ```

        - Shutdown all unused interfaces:
        ```
            interface range FastEthernet0/5-24
                shutdown
        ```
        ```
            end
            write memory
        ```   

### PCs Configuration:
    **Time:** 10 min
    **Configuration**:
        - Admin_PC:
            IPv4 Adress: 192.168.10.11
            Subnet mask: 255.255.255.0
            Default gateway: 192.168.10.1
            Dns server: 0.0.0.0

        - Stuff_PC:
            IPv4 Adress: 192.168.20.11
            Subnet mask: 255.255.255.0
            Default gateway: 192.168.20.1
            Dns server: 0.0.0.0

        - Guest_PC:
            IPv4 Adress: 192.168.30.11
            Subnet mask: 255.255.255.0
            Default gateway: 192.168.30.1
            Dns server: 0.0.0.0

---

## Key learning:
    1. How to create and configurate VLANs and there roles.
    2. How to make VLANs communicate each other.
    3. The role of DHCP and what range should be excluded for servers and IT/Admin members.

---

## Screenshots:
    - [x] router_config.png
    - [x] switch_config.png
    - [x] pc_ping.png
    - [x] topology_final.png

## Metrics:
    **Total Time:** 75 min
    **Status:** COMPLETE

