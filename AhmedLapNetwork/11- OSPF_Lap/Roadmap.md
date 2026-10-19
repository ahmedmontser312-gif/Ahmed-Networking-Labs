
2. Method
Step 1 — Configure the PCs
LAN 1
PC1 → 192.168.1.1
PC0 → 192.168.1.2
Gateway → 192.168.1.254
LAN 2
PC2 → 192.168.2.1
PC3 → 192.168.2.2
Gateway → 192.168.2.254
LAN 3
PC4 → 192.168.3.1
PC5 → 192.168.3.2
Gateway → 192.168.3.254
Subnet Mask:
255.255.255.0
Step 2 — Configure R1
LAN interface:
enable
configure terminal

interface <LAN-interface>
ip address 192.168.1.254 255.255.255.0
no shutdown
exit
R1 → R2:
interface <R1-R2-interface>
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
R1 → R3:
interface <R1-R3-interface>
ip address 192.168.30.1 255.255.255.0
no shutdown
exit
Step 3 — Configure R2
LAN interface:
enable
configure terminal

interface <LAN-interface>
ip address 192.168.2.254 255.255.255.0
no shutdown
exit
R2 → R1:
interface <R2-R1-interface>
ip address 192.168.10.2 255.255.255.0
no shutdown
exit
R2 → R3:
interface <R2-R3-interface>
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
Step 4 — Configure R3
LAN interface:
enable
configure terminal

interface <LAN-interface>
ip address 192.168.3.254 255.255.255.0
no shutdown
exit
R3 → R2:
interface <R3-R2-interface>
ip address 192.168.20.2 255.255.255.0
no shutdown
exit
R3 → R1:
interface <R3-R1-interface>
ip address 192.168.30.2 255.255.255.0
no shutdown
exit
Note: Interface names may differ depending on the router model. Use show ip interface brief to identify the available interfaces.
Step 5 — Configure OSPF on R1
router ospf 1
router-id 1.1.1.1

network 192.168.1.0 0.0.0.255 area 0
network 192.168.10.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0

end
Step 6 — Configure OSPF on R2
router ospf 1
router-id 2.2.2.2

network 192.168.2.0 0.0.0.255 area 0
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0

end
Step 7 — Configure OSPF on R3
router ospf 1
router-id 3.3.3.3

network 192.168.3.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0

end
Step 8 — Verify OSPF
On each router:
show ip ospf neighbor
Then:
show ip route
Look for routes marked with:
O
These are routes learned through OSPF.
Step 9 — Test Connectivity
From PC1:
ping 192.168.2.1
Then:
ping 192.168.3.1
From PC4:
ping 192.168.1.1
✅ Final Goal
All PCs in 192.168.1.0/24, 192.168.2.0/24, and 192.168.3.0/24 should be able to communicate with each other through OSPF.