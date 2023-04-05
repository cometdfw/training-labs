---
title: "vPC Part 2: vPC Implementation"
layout: post
---

> In this section, you will implement the feature of Virtual Port-Channel (vPC) on the Nexus switches.

The workflow to implement vPC:

- Enable necessary features
- Define vPC domain
- Establish peer keep-alive link
- Establish vPC peer link
- Add vPC member port

## 1. Enable LACP and vPC features

Enable LACP and vPC feature on both switches, where LACP is the necessary feature to form link aggregation group (LAG) for Port-Channel.

- On N9K-1:

```
N9K-1(config)# feature lacp
N9K-1(config)# feature vpc
```

- On N9K-2:

```
N9K-2(config)# feature lacp
N9K-2(config)# feature vpc
```



## 2. Define vPC domain and Establish peer keep-alive link

The vPC domain is the domain configured across two vPC peer devices. The vPC domain ID can be between 1-1000. We are using 1 as the vPC domain ID in this lab.

This keep-alive link is used to determine if the peer link or the vPC peer is down or failed. We are using the management interfaces for the keep alive link in this lab. You can use other interfaces for the keep alive link if you want.

- On N9K-1:

```
N9K-1(config)# vpc domain 1
N9K-1(config-vpc-domain)# peer-keepalive destination 10.0.0.2 source 10.0.0.1 vrf management
```

- On N9K-2:

```
N9K-2(config)# vpc domain 1
N9K-2(config-vpc-domain)# peer-keepalive destination 10.0.0.1 source 10.0.0.2 vrf management
```



## 3. Establish vPC peer link

This link is used to synchronize the state and carries control traffic between the vPC peer devices.

- On N9K-1:

```
N9K-1(config)# interface e1/2 - 3
N9K-1(config-if-range)# channel-group 1 mode active
N9K-1(config-if-range)# exit
N9K-1(config)#
N9K-1(config)# interface port-channel1
N9K-1(config-if)# switchport
N9K-1(config-if)# switchport mode trunk
N9K-1(config-if)# vpc peer-link
```

- On N9K-2:

```
N9K-2(config)# interface e1/2 - 3
N9K-2(config-if-range)# channel-group 1 mode active
N9K-2(config-if-range)# exit
N9K-2(config)#
N9K-1(config)# interface port-channel1
N9K-2(config-if)# switchport
N9K-2(config-if)# switchport mode trunk
N9K-2(config-if)# vpc peer-link
```



## 4. Add vPC member port

Member port is where downstream device is connected to. Member port indicates the member of particular vPCs configured on the vPC peers.

- On N9K-1:

```
N9K-1(config)# interface e1/1
N9K-1(config-if)# channel-group 11 mode active
N9K-1(config-if)# exit
N9K-1(config)#
N9K-1(config)# interface port-channel11
N9K-1(config-if)# switchport
N9K-1(config-if)# switchport mode access
N9K-1(config-if)# switchport access vlan 10
N9K-1(config-if)# vpc 11
```

- On N9K-2:

```
N9K-2(config)# interface e1/1
N9K-2(config-if)# channel-group 11 mode active
N9K-2(config-if)# exit
N9K-2(config-if)#
N9K-2(config)# interface port-channel11
N9K-2(config-if)# switchport
N9K-2(config-if)# switchport mode access
N9K-2(config-if)# switchport access vlan 10
N9K-2(config-if)# vpc 11
```

This concludes the **vPC Part 2: vPC Implementation**. Please continue with **vPC Part 3: Downstream Device Configs**.
