---
title: "vPC Part 1: vPC Peer Base Configs"
layout: post
---

> In this section, you will configure the host name and management IP address for the Nexus switches.

## 1. Switch Access Information

| **Device Name**             | **Mgmt IP Address**      | **Username / password**      |
|-----------------------------|--------------------------|------------------------------|
| N9K-1                       | 10.0.0.1                 | cisco / cisco                |
| N9K-2                       | 10.0.0.2                 | cisco / cisco                |

We will configure the device name and management IP addresses for the switches in the later part of the lab practice. You can access the devices with username **cisco** and password **cisco**.


## 2. Configure host name and management IP address

**Step 1**. Console into **N9K-1** and **N9K-2** with username **cisco** and password **cisco**.

**Step 2**. Configure hostname on the **N9K-1** and **N9K-2** switches.

- On N9K-1:

```
switch# conf t
Enter configuration commands, one per line. End with CNTL/Z.
switch(config)# hostname N9K-1
N9K-1(config)#
```

- On N9K-2:

```
switch# conf t
Enter configuration commands, one per line. End with CNTL/Z.
switch(config)# hostname N9K-2
N9K-2(config)#
```

**Step 3**. Configure management IP address for the **N9K-1** and **N9K-2** switches.

- On N9K-1:

```
N9K-1(config)# interface mgmt 0
N9K-1(config-if)# ip address 10.0.0.1/30
N9K-1(config-if)# no shutdown
```

- On N9K-2:

```
N9K-2(config)# interface mgmt 0
N9K-2(config-if)# ip address 10.0.0.2/30
N9K-2(config-if)# no shutdown
```

This concludes the **vPC Part 1: vPC Peer Base Configs**. Please continue with **vPC Part 2: vPC Implementation**.
