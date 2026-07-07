# My 1st Main Lab Project Practice

This is my first main project working with Cisco Packet Tracer, apart from regular lab exercises. Though limited features and lacking in more security and traffic control/flow protocols, I'd say this is the best output I've worked with so far. Not an expert, but definitely learning progressively.

---

# Certification/Training Center LAN & WAN

## 1. OVERVIEW

This project I aimed to simulate a small-to-medium business/company, that is Certifications & Training Center Company. This mainly demonstrates implementations of design that is "supposed to be" secure, scalable and highly available enterprise network. This is only a simulation of the real thing, but makes use of multiple departments, connectivity, services and communication while maintaining fault tolerance, just like in real-world business. However, I do understand it's much more harsh, stricter than this project.

---

## 2. MY OBJECTIVES

- Design scalable campus network(departments)
- Redundancy/Availability
- Try to implement Zero-Trust network security using ACL + Firewall
- Simulate internet(Cloud) connectivity
- Explore possible protocols/policies I can use

---

## 3. LOGICAL TOPOLOGY

The company's network design uses Hierarchal Campus network design using Collapsed-core. As you can see, the SW8-CORE acts as the centralized routing and distribution point for all access layer switches. On the WAN side, it shows WAN connectivity, security and redundancy using HSRP protocols, Firewall Policies, and ISP connectivity.

Noticably, each Lab VLAN only contains 1 PC each. Normally, I would show at least 5-10 to represent a Lab Room but it's simply just to represent and wouldn't change anything regardless.

---

## 4. NETWORK DEVICES/CONCEPTs/PROTOCOLS/FEATURES

### a. Access Layer Switches

**DESCRIPTION**

- Consists of 8 Layer 2 Access Switches (7 from Internal, 1 from External)
- Each switch handles EXACTLY 1 VLAN represented as a Lab Room

---

### b. Core Layer Switch

**DESCRIPTION**

- Right now, it is the central routing and distribution device and allows for interVLAN communication.
- It is also where the ACL rules are implemented, not in each Layer 2 switch.

**FLAW/s**

- It is noticable that it is an aggregation point for all access switches and can handle, but in terms of scalability measures, I do acknowledge that it does need another Layer 3 switch to pair with to reduce bottlenecking, especially towards the interface of WAN side. That WAN interface remains a single point of failure in the instance that interface goes down. However, I will hold it off for now.

---

### c. ACL

**DESCRIPTION**

- As seen in "ACLRules.PNG", it is designed to allow interVLAN traffic in very specific ways.
- VLANs 10,20,30,40 are allowed to access the LMS &FTP Servers via Web or IP.
- VLAN 50 is NOT ALLOWED any access except its own gateway.
- VLAN 90, on the other hand, is only allowed FTP service.
- All internal network VLAN PCs are serviced with DHCP IPs, and allowed those who are allowed to access using domain names, especially for LMS & FTP.
- That's it for the internal network rules.

**FLAW/s**

- However, one major misconfiguration I have not been able to figure out yet is that once I remove a single line from the Access List, the connection of that line still works since it's being catched and accomodated with by the last line "permit ip any any" and is not denied.

---

### d. Firewall

**DESCRIPTION**

- The policy includes treating any internal network traffic to have the HIGHEST security level of 100, trusting traffic to come from INSIDE to OUTSIDE.
- On the other hand, incoming outside traffic is considered security level of 0.

The main ACL rules for OUTSIDE incoming traffic are the following:

**PERMIT**

- If a packet is an echo-reply/error messages replying to INTERNAL

**DENY**

- EVERYTHING ELSE

**FLAW**

- The goal of Zero-Trust network is that treat both INCOMING and OUTCOMING traffic with ZERO TRUST and must be treated as unsecured as I've learned from real-world businesses. I didn't realize that until I thoroughly checked and got advice from my professor.

---

### e. HSRP Routers + NAT

**DESCRIPTION**

- NAT was configured on the 2 HSRP routers instead since they are leaving the network towards Cloud to convert internal IP addresses to a public one.
- ACL Rules also applied here.

**FLAW**

- Another major flaw is that the Active Router is currently on preempt, but once the interface towards the ISP router is down, an outcoming packet will still go to the Active Router despite the next interface being down.
- One feedback I got from our professor is that recommended Interface Tracking allowing to monitor the status of its own upstream interface, in which if it doesn't it automatically lowers its HSRP priority and allow the Standby Router to assume Active role.

---

### f. ISP Routers

**DESCRIPTION**

- The reason there is a separate router between the HSRP Routers and the Cloud interfaces is since we're simulating a real-world implementation.
- HSRP routers don't directly connect immediately to the internet, instead connect to ISP Routers that are NOT OWNED by the company, also the reason why the NAT is configured on HSRP Routers instead.

---

### g. Cloud Interfaces

**DESCRIPTION**

- These are simulated mediums instead and don't have addressed interfaces.
- All I configured in this interface is add connection between Modem to Ethernet interfaces.
- It will bridge connection between ISP Routers and the DSL Modem.

---

### h. Modem

**DESCRIPTION**

- Like a hub, simply forward traffic but translates Ethernet frames from the EXTERNAL network and converts it to suiitable signal over the Telephone cable.

---

### i. External Network

**DESCRIPTION**

- As I have said before, the External Network represents the certifying body's network server where students get certificates from after confiriming their progress, or at least that's the concept of it.
- A DNS Server was also added so that PCs from Lab VLANs can access the domain instead of manual IP address.

---

## 5. FUTURE IMPROVEMENTS

### a. Single Points of Failure

- Exists in Interfaces between SW8-Core and Firewall and the rest of the WAN side.

### b. Simulation Environment

- It's not validated against real-world operation conditions especially I mainly focused on the logical side, not the physical topology side.
- No live attacks, failovers, physical issues can be tested on this simulation.

### c. EtherChannel instead of Dual-Link / Availability Issues

- Since each Layer 3 switch can only accomodate 6 port channels in which case in our topology, we have more than that.
- However, it is considerable to know that the VLAN Labs are better off EtherChanneled and the rest of the ports are normal STP-based dual links instead to at least demonstrate its use for higher availability.
- The same can be said for Firewall redundancy.

### d. Adopt more advanced Security/Monitoring solutions

---

# TAKEAWAYS

As I work through my project until I create this write-up I learned there are a lot more things to be considered when making network system design, especially for higher scale enterprise networks where downtime is crucial, availability is of highest priority, and security should be innegligible.

This simulation is only surface-level compared to real network systems, so I plan to upskill and practice and learn more as I go through.
