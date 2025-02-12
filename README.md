# high-availability-network-configuration
A comprehensive guide and configuration examples for setting up high availability using VRRP (Virtual Router Redundancy Protocol) with Cisco routers.

High Availability Network Configuration Guide

Overview

This guide demonstrates how to set up a High Availability (HA) network configuration using Virtual Router Redundancy Protocol (VRRP). The goal of this configuration is to ensure that if one router fails, another router takes over, minimizing downtime and ensuring continuous network service.

In this configuration, two routers are configured to use VRRP, and if the primary router fails, the secondary router automatically takes over the IP address of the default gateway.

Requirements
	•	Two routers (Router 1 and Router 2)
	•	VRRP enabled on both routers.
	•	Static IP addresses assigned to the routers.
	•	Access to both routers via their management interfaces.

Network Topology
[LAN] ---
[Router 1 (Primary)]--- 
[Router 2 (Secondary)] ---
[Default Gateway]   
[Default Gateway]

Step 1: Configure Router 1 (Primary Router)

1.	Log in to the router.
2.	Enter the configuration mode:
   ---Router1# configure terminal

3.	Set the VRRP Group and assign the virtual IP address:
---Router1(config)# interface GigabitEthernet0/0
---Router1(config-if)# vrrp 1 ip 192.168.1.1
---Router1(config-if)# vrrp 1 priority 110
---Router1(config-if)# vrrp 1 preempt
---Router1(config-if)# exit

•	vrrp 1 ip 192.168.1.1: Sets the virtual IP address.
•	priority 110: Assigns a higher priority to Router 1 to make it the preferred router.
•	preempt: Allows Router 1 to take over as the master if it comes back online after a failure.

Step 2: Configure Router 2 (Secondary Router)

1.	Log in to the second router.
2.	Enter the configuration mode:
---Router2# configure terminal

3.	Set the VRRP Group and assign the virtual IP address:

---Router2(config)# interface GigabitEthernet0/0
---Router2(config-if)# vrrp 1 ip 192.168.1.1
---Router2(config-if)# vrrp 1 priority 100
---Router2(config-if)# vrrp 1 preempt
---Router2(config-if)# exit

•	priority 100: Assigns a lower priority to Router 2 so that Router 1 will be the preferred master router.
•	preempt: Allows Router 2 to take over if Router 1 fails.

Step 3: Verify the Configuration

1.	To check the VRRP status on Router 1:
---Router1# show vrrp
2.	To check the VRRP status on Router 2:
---Router2# show vrrp

These commands will display which router is the master and whether the virtual IP address is correctly configured.

Step 4: Test the Configuration

To simulate a failure, you can disable the interface on Router 1 and check if Router 2 takes over the virtual IP address.

1.	Disable the interface on Router 1:

---Router1# interface GigabitEthernet0/0
---Router1(config-if)# shutdown

2.	Check if Router 2 becomes the master router by running the following on Router 2:
---Router2# show vrrp

Conclusion

This configuration ensures that if one router fails, the other router will take over the virtual IP address and continue routing traffic, ensuring high availability and minimizing downtime. This is crucial for network reliability in enterprise environments.

