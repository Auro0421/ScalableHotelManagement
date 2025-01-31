**SCALABLE HOTEL MANAGEMENT:**

This project focuses on designing and implementing a robust network infrastructure for a hotel with multiple departments. The network utilizes VLANs to optimize communication, enhance security, and efficiently manage resources within the organization. Each department operates on a dedicated VLAN, while a separate VLAN is designated for guests, ensuring isolation from internal operations.

**Network Design and Architecture:** The network is segmented into five VLANs:

- **VLAN 10:** Reception
- **VLAN 20:** Finance
- **VLAN 30:** Restaurant
- **VLAN 40:** Sales
- **VLAN 50:** Guest (isolated for guest PCs)

Each VLAN corresponds to a specific hotel department, ensuring organized network segmentation. The guest VLAN is secured using Access Control Lists (ACLs) to prevent unauthorized access to internal systems, while internal departments maintain controlled inter-communication as required.

### **Core Network Components:**

- **Switches:** Manage VLAN segmentation for different departments.
- **Router:** Facilitates inter-VLAN communication.
- **Access Control Lists (ACLs):** Regulate traffic flow between VLANs, enhancing security.
- **IP Addressing Scheme:** Defines structured IP distribution for seamless communication.

#### **IP Addressing Scheme:**

| VLAN | Department | IP Range                    | Subnet Mask   | Gateway      |
| ---- | ---------- | --------------------------- | ------------- | ------------ |
| 10   | Reception  | 192.168.10.0 - 192.168.10.3 | 255.255.255.0 | 192.168.10.1 |
| 20   | Finance    | 192.168.20.0 - 192.168.20.3 | 255.255.255.0 | 192.168.20.1 |
| 30   | Restaurant | 192.168.30.0 - 192.168.30.3 | 255.255.255.0 | 192.168.30.1 |
| 40   | Sales      | 192.168.40.0 - 192.168.40.3 | 255.255.255.0 | 192.168.40.1 |
| 50   | Guest      | 192.168.50.0 - 192.168.50.3 | 255.255.255.0 | 192.168.50.1 |

Each department is assigned a VLAN with three statically allocated PCs:

#### **PC IP Assignments:**

- **Reception (VLAN 10):**

  - PC1: 192.168.10.2
  - PC2: 192.168.10.3
  - PC3: 192.168.10.4

- **Finance (VLAN 20):**

  - PC1: 192.168.20.2
  - PC2: 192.168.20.3
  - PC3: 192.168.20.4

- **Restaurant (VLAN 30):**

  - PC1: 192.168.30.2
  - PC2: 192.168.30.3
  - PC3: 192.168.30.4

- **Sales (VLAN 40):**

  - PC1: 192.168.40.2
  - PC2: 192.168.40.3
  - PC3: 192.168.40.4

- **Guest (VLAN 50):**

  - PC1: 192.168.50.2
  - PC2: 192.168.50.3
  - PC3: 192.168.50.4

### **Switch Configuration:**

The VLANs are configured as follows:

```
vlan 10 name Reception
vlan 20 name Finance
vlan 30 name Restaurant
vlan 40 name Sales
vlan 50 name Guest
```

The VLANs are assigned to the appropriate ports:

```
interface range fastethernet 0/2 - 0/4
 switchport mode access
 switchport access vlan 10 // Reception VLAN

interface range fastethernet 0/5 - 0/7
 switchport mode access
 switchport access vlan 20 // Finance VLAN

interface range fastethernet 0/8 - 0/10
 switchport mode access
 switchport access vlan 30 // Restaurant VLAN

interface range fastethernet 0/11 - 0/13
 switchport mode access
 switchport access vlan 40 // Sales VLAN

interface range fastethernet 0/14 - 0/16
 switchport mode access
 switchport access vlan 50 // Guest VLAN
```

### **Router Configuration:**

The router enables inter-VLAN communication by implementing sub-interfaces for each VLAN:

```
interface gig0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface gig0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface gig0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

interface gig0/0.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0

interface gig0/0.50
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
 ip access-group 101 in
```

Each sub-interface acts as the gateway for its respective VLAN, allowing communication between internal departments while ensuring proper traffic control.

### **Access Control Lists (ACLs):**

To maintain network security, ACLs are deployed to restrict access from the guest VLAN to internal VLANs:

```
access-list 101 deny ip 192.168.10.0 0.0.0.255 192.168.50.0 0.0.0.255
access-list 101 deny ip 192.168.20.0 0.0.0.255 192.168.50.0 0.0.0.255
access-list 101 deny ip 192.168.30.0 0.0.0.255 192.168.50.0 0.0.0.255
access-list 101 deny ip 192.168.40.0 0.0.0.255 192.168.50.0 0.0.0.255
access-list 101 permit ip any any
```

This configuration ensures that guest users cannot access sensitive internal resources while allowing unrestricted communication among internal VLANs.

### **Conclusion:**

This hotel network infrastructure provides a secure, efficient, and scalable solution for departmental communication. The use of VLANs enhances network performance and security, while ACLs ensure data integrity by isolating guest traffic. The design is future-proof, allowing seamless scalability as the hotel grows.

