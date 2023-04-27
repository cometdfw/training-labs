---
title: "vPC Lab Section 3: Downstream Switch Configs"
layout: post
---

> In this section, you will create Port-Channel on the downstream switch.

 vPC will not be up until the corresponding Port-Cannel is created on the downstream device.

## 1. Configure host name on the downstream switch

**Step 1**. Console into **N9K-3** with username **cisco** and password **cisco**.

**Step 2**. Configure hostname on the **N9K-3** switch.

- On N9K-3:

```
switch# conf t
Enter configuration commands, one per line. End with CNTL/Z.
switch(config)# hostname N9K-3
N9K-3(config)#
```

## 2. Enable VLAN

- On N9K-3:

```
N9K-3(config)# vlan 11
```

## 3. Enable LACP feature

- On N9K-3:

```
N9K-3(config)# feature lacp
```

## 4. Create Port-Channel

- On N9K-3:

```
N9K-1(config)# interface e1/1 - 2
N9K-1(config-if)# channel-group 11 mode active
N9K-1(config-if)# exit
N9K-1(config)#
N9K-1(config)# interface port-channel11
N9K-1(config-if)# switchport
N9K-1(config-if)# switchport mode access
N9K-1(config-if)# switchport access vlan 11
```



This concludes the **Section 3: Downstream Switch Configs**. Please continue with [**Section 4: vPC Verification**](https://cometdfw.github.io/training-labs/vpc-04-vpc-verification/).
