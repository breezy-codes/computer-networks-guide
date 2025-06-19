# 5 – Inter-VLAN Routing with Trunking (Router-on-a-Stick)

This tutorial builds on the topology and configuration created in [Tutorial 4](../cisco/tutorial-series/tutorial4), where inter-VLAN routing was implemented using **separate physical interfaces** on the router. In this tutorial, we’ll upgrade the design to a more scalable and industry-standard approach by configuring **Router-on-a-Stick (ROAS)** using a **trunk link** and **router subinterfaces**.

Using trunking simplifies physical cabling and allows a single router interface to route traffic for **multiple VLANs**, making it more efficient and closer to how enterprise networks are deployed.

## Objectives

* Reuse the network design from Tutorial 4
* Reconfigure the switch-router links as trunk ports
* Configure a single router interface with subinterfaces for VLAN 10 and VLAN 20
* Verify inter-VLAN routing using trunking

---

## Part 1 – Review of Existing Topology

We will reuse the topology from Tutorial 4:

```bash
           +---------+          +---------+          +---------+
PC1 -------|         |          |         |          |         |------- PC3
           | Switch0 |--- fa0/24| Router0 |fa0/24 ---| Switch1 |
PC2 -------|         |          |         |          |         |------- PC4
           +---------+          +---------+          +---------+
```

* **PC1 & PC2** are in **VLAN 10** (Department A)
* **PC3 & PC4** are in **VLAN 20** (Department B)
* **Switch0** and **Switch1** are connected to **Router0** via `fa0/24`

> **Important Change:** Instead of using separate physical interfaces on the router for each VLAN, both switches will now connect to **GigabitEthernet0/0** on Router0. This interface will carry **trunked traffic** for both VLANs.

---

## Part 2 – Switch Configuration (Trunking)

### Step 2.1 – Delete Previous Access Mode Configuration (if needed)

If your `fa0/24` ports were previously configured as access ports (from Tutorial 4), remove those settings:

```bash
configure terminal
interface fa0/24
no switchport access vlan
no switchport mode access
exit
```

### Step 2.2 – Configure Trunk Ports on Both Switches

```bash
interface fa0/24
switchport trunk encapsulation dot1q
switchport mode trunk
exit
```

Repeat this on both **Switch0** and **Switch1**.

> **dot1Q** is the industry-standard protocol for VLAN tagging over trunk links.

### Step 2.3 – Verify Trunking

Use this command on each switch:

```bash
show interfaces trunk
```

You should see `fa0/24` listed as a trunk port with VLANs allowed.

---

## Part 3 – Router Subinterface Configuration

We will now configure **GigabitEthernet0/0** on the router with **subinterfaces** for each VLAN.

### Step 3.1 – Configure VLAN 10 Subinterface

```bash
interface gig0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
description Dept_A_Gateway
no shutdown
exit
```

### Step 3.2 – Configure VLAN 20 Subinterface

```bash
interface gig0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
description Dept_B_Gateway
no shutdown
exit
```

### Step 3.3 – Enable the Physical Interface

```bash
interface gig0/0
no shutdown
exit
```

### Step 3.4 – Save Configuration

```bash
end
write memory
```

---

## Part 4 – PC Configuration (Static IPs)

Assign the same IP configurations from Tutorial 4:

| PC  | VLAN    | IP Address    | Subnet Mask   | Default Gateway |
| --- | ------- | ------------- | ------------- | --------------- |
| PC1 | VLAN 10 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1    |
| PC2 | VLAN 10 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1    |
| PC3 | VLAN 20 | 192.168.20.13 | 255.255.255.0 | 192.168.20.1    |
| PC4 | VLAN 20 | 192.168.20.14 | 255.255.255.0 | 192.168.20.1    |

> Use Desktop > IP Configuration on each PC to apply these settings.

---

## Part 5 – Verification and Troubleshooting

### Step 5.1 – Test Inter-VLAN Routing

* Ping **PC1 to PC3**: `ping 192.168.20.13`
* Ping **PC2 to PC4**: `ping 192.168.20.14`
* Ping **PC3 to PC1**: `ping 192.168.10.11`

All should succeed if trunking and routing are configured properly.

### Step 5.2 – Troubleshooting Commands

#### On Switches:

```bash
show vlan brief
show interfaces trunk
```

#### On Router:

```bash
show ip interface brief
show running-config
```

If pings fail, ensure:

* Trunking is enabled on fa0/24
* Router subinterfaces are correctly configured
* PCs have correct IP and default gateway

---

## Summary

In this tutorial, you:

* Converted your previous inter-VLAN routing setup to use **router-on-a-stick** with **802.1Q trunking**
* Configured **subinterfaces** for each VLAN on a single router interface
* Verified end-to-end connectivity across VLANs
