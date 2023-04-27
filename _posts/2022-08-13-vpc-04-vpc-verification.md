---
title: "vPC Lab Section 4: vPC Verification"
layout: post
---

> In this section, you will verify the feature of Virtual Port-Channel (vPC) from the Nexus switches.

## 1. Verify vPC peer link

Use **show vpc brief** or **show vpc** command to verify the vPC domain ID, the vPC peer and keep-alive message status, the configuration consistency status, the vPC role, the vPC Peer-Link status, and the Port-Channel as well as the corresponding VLAN(s).

- On N9K-1:

```
N9K-1# show vpc brief
Legend:
                (*) - local vPC is down, forwarding via vPC peer-link

vPC domain id                     : 1   
Peer status                       : peer adjacency formed ok      
vPC keep-alive status             : peer is alive                 
Configuration consistency status  : success
Per-vlan consistency status       : success                       
Type-2 consistency status         : success
vPC role                          : primary                       
Number of vPCs configured         : 1   
Peer Gateway                      : Disabled
Dual-active excluded VLANs        : -
Graceful Consistency Check        : Enabled
Auto-recovery status              : Disabled
Delay-restore status              : Timer is off.(timeout = 30s)
Delay-restore SVI status          : Timer is off.(timeout = 10s)
Operational Layer3 Peer-router    : Disabled
Virtual-peerlink mode             : Disabled

vPC Peer-link status
---------------------------------------------------------------------
id    Port   Status Active vlans    
--    ----   ------ -------------------------------------------------
1     Po1    up     1,11                                                        


vPC status
----------------------------------------------------------------------------
Id    Port          Status Consistency Reason                Active vlans
--    ------------  ------ ----------- ------                ---------------
11    Po11          up     success     success               11                 

<omitted>
```

- On N9K-2:

```
N9K-2# show vpc brief
Legend:
                (*) - local vPC is down, forwarding via vPC peer-link

vPC domain id                     : 1   
Peer status                       : peer adjacency formed ok      
vPC keep-alive status             : peer is alive                 
Configuration consistency status  : success
Per-vlan consistency status       : success                       
Type-2 consistency status         : success
vPC role                          : secondary                     
Number of vPCs configured         : 1   
Peer Gateway                      : Disabled
Dual-active excluded VLANs        : -
Graceful Consistency Check        : Enabled
Auto-recovery status              : Disabled
Delay-restore status              : Timer is off.(timeout = 30s)
Delay-restore SVI status          : Timer is off.(timeout = 10s)
Operational Layer3 Peer-router    : Disabled
Virtual-peerlink mode             : Disabled

vPC Peer-link status
---------------------------------------------------------------------
id    Port   Status Active vlans    
--    ----   ------ -------------------------------------------------
1     Po1    up     1,11                                                        


vPC status
----------------------------------------------------------------------------
Id    Port          Status Consistency Reason                Active vlans
--    ------------  ------ ----------- ------                ---------------
11    Po11          up     success     success               11                 

<omitted>                                                                                
```

## 2. Check vPC peer keep-alive link in details

- On N9K-1:

```
N9K-1# show vpc peer-keepalive

vPC keep-alive status             : peer is alive                 
--Peer is alive for             : (6459) seconds, (378) msec
--Send status                   : Success
--Last send at                  : 2023.04.04 19:20:57 700 ms
--Sent on interface             : mgmt0
--Receive status                : Success
--Last receive at               : 2023.04.04 19:20:57 619 ms
--Received on interface         : mgmt0
--Last update from peer         : (0) seconds, (591) msec

vPC Keep-alive parameters
--Destination                   : 10.0.0.2
--Keepalive interval            : 1000 msec
--Keepalive timeout             : 5 seconds
--Keepalive hold timeout        : 3 seconds
--Keepalive vrf                 : management
--Keepalive udp port            : 3200
--Keepalive tos                 : 192
```

- On N9K-2:

```
N9K-2# show vpc peer-keepalive

vPC keep-alive status             : peer is alive                 
--Peer is alive for             : (6457) seconds, (43) msec
--Send status                   : Success
--Last send at                  : 2023.04.04 19:20:53 508 ms
--Sent on interface             : mgmt0
--Receive status                : Success
--Last receive at               : 2023.04.04 19:20:53 599 ms
--Received on interface         : mgmt0
--Last update from peer         : (0) seconds, (779) msec

vPC Keep-alive parameters
--Destination                   : 10.0.0.1
--Keepalive interval            : 1000 msec
--Keepalive timeout             : 5 seconds
--Keepalive hold timeout        : 3 seconds
--Keepalive vrf                 : management
--Keepalive udp port            : 3200
--Keepalive tos                 : 192
```

## 3. Check vPC consistency in details

- On N9K-1:

```
N9K-1# show vpc consistency-parameters global

    Legend:
        Type 1 : vPC will be suspended in case of mismatch

Name                        Type  Local Value            Peer Value             
-------------               ----  ---------------------- -----------------------
STP MST Simulate PVST       1     Enabled                Enabled               
STP Port Type, Edge         1     Normal, Disabled,      Normal, Disabled,     
BPDUFilter, Edge BPDUGuard        Disabled               Disabled              
STP MST Region Name         1     ""                     ""                    
STP Disabled                1     None                   None                  
STP Mode                    1     Rapid-PVST             Rapid-PVST            
STP Bridge Assurance        1     Enabled                Enabled               
STP Loopguard               1     Disabled               Disabled              
STP MST Region Instance to  1                                                  
 VLAN Mapping                                                                  
STP MST Region Revision     1     0                      0                     
Xconnect Vlans              1                                                  
QoS (Cos)                   2     ([0-7], [], [], [],    ([0-7], [], [], [],   
                                  [], [], [], [])        [], [], [], [])       
Network QoS (MTU)           2     (1500, 1500, 1500,     (1500, 1500, 1500,    
                                  1500, 0, 0, 0, 0)      1500, 0, 0, 0, 0)     
Network Qos (Pause:         2     (F, F, F, F, F, F, F,  (F, F, F, F, F, F, F,
T->Enabled, F->Disabled)          F)                     F)                    
Input Queuing (Bandwidth)   2     (0, 0, 0, 0, 0, 0, 0,  (0, 0, 0, 0, 0, 0, 0,
                                  0)                     0)                    
Input Queuing (Absolute     2     (F, F, F, F, F, F, F,  (F, F, F, F, F, F, F,
Priority: T->Enabled,             F)                     F)                    
F->Disabled)                                                                   
Output Queuing (Bandwidth   2     (100, 0, 0, 0, 0, 0,   (100, 0, 0, 0, 0, 0,  
Remaining)                        0, 0)                  0, 0)                 
Output Queuing (Absolute    2     (F, F, F, T, F, F, F,  (F, F, F, T, F, F, F,
Priority: T->Enabled,             F)                     F)                    
F->Disabled)                                                                   
Allowed VLANs               -     1,11                   1,11                  
Local suspended VLANs       -     -                      -                     
```

- On N9K-2:

```
N9K-2# show vpc consistency-parameters global

    Legend:
        Type 1 : vPC will be suspended in case of mismatch

Name                        Type  Local Value            Peer Value             
-------------               ----  ---------------------- -----------------------
STP MST Simulate PVST       1     Enabled                Enabled               
STP Port Type, Edge         1     Normal, Disabled,      Normal, Disabled,     
BPDUFilter, Edge BPDUGuard        Disabled               Disabled              
STP MST Region Name         1     ""                     ""                    
STP Disabled                1     None                   None                  
STP Mode                    1     Rapid-PVST             Rapid-PVST            
STP Bridge Assurance        1     Enabled                Enabled               
STP Loopguard               1     Disabled               Disabled              
STP MST Region Instance to  1                                                  
 VLAN Mapping                                                                  
STP MST Region Revision     1     0                      0                     
Xconnect Vlans              1                                                  
QoS (Cos)                   2     ([0-7], [], [], [],    ([0-7], [], [], [],   
                                  [], [], [], [])        [], [], [], [])       
Network QoS (MTU)           2     (1500, 1500, 1500,     (1500, 1500, 1500,    
                                  1500, 0, 0, 0, 0)      1500, 0, 0, 0, 0)     
Network Qos (Pause:         2     (F, F, F, F, F, F, F,  (F, F, F, F, F, F, F,
T->Enabled, F->Disabled)          F)                     F)                    
Input Queuing (Bandwidth)   2     (0, 0, 0, 0, 0, 0, 0,  (0, 0, 0, 0, 0, 0, 0,
                                  0)                     0)                    
Input Queuing (Absolute     2     (F, F, F, F, F, F, F,  (F, F, F, F, F, F, F,
Priority: T->Enabled,             F)                     F)                    
F->Disabled)                                                                   
Output Queuing (Bandwidth   2     (100, 0, 0, 0, 0, 0,   (100, 0, 0, 0, 0, 0,  
Remaining)                        0, 0)                  0, 0)                 
Output Queuing (Absolute    2     (F, F, F, T, F, F, F,  (F, F, F, T, F, F, F,
Priority: T->Enabled,             F)                     F)                    
F->Disabled)                                                                   
Allowed VLANs               -     1,11                   1,11                  
Local suspended VLANs       -     -                      -                     
```

## 4. Check vPC role in details

- On N9K-1:

```
N9K-1# show vpc role

vPC Role status
----------------------------------------------------
vPC role                        : primary                       
Dual Active Detection Status    : 0
vPC system-mac                  : 00:23:04:ee:be:01             
vPC system-priority             : 32667
vPC local system-mac            : 52:00:00:06:1b:08             
vPC local role-priority         : 32667
vPC local config role-priority  : 32667
vPC peer system-mac             : 52:00:00:0c:1b:08             
vPC peer role-priority          : 32667
vPC peer config role-priority   : 32667
```

- On N9K-2:

```
N9K-2# show vpc role

vPC Role status
----------------------------------------------------
vPC role                        : secondary                     
Dual Active Detection Status    : 0
vPC system-mac                  : 00:23:04:ee:be:01             
vPC system-priority             : 32667
vPC local system-mac            : 52:00:00:0c:1b:08             
vPC local role-priority         : 32667
vPC local config role-priority  : 32667
vPC peer system-mac             : 52:00:00:06:1b:08             
vPC peer role-priority          : 32667
vPC peer config role-priority   : 32667
```

## 5. Verify Port-Channel status and member ports

- On N9K-1:

```
N9K-1# show port-channel summary
Flags:  D - Down        P - Up in port-channel (members)
        I - Individual  H - Hot-standby (LACP only)
        s - Suspended   r - Module-removed
        b - BFD Session Wait
        S - Switched    R - Routed
        U - Up (port-channel)
        p - Up in delay-lacp mode (member)
        M - Not in use. Min-links not met
--------------------------------------------------------------------------------
Group Port-       Type     Protocol  Member Ports
      Channel
--------------------------------------------------------------------------------
1     Po1(SU)     Eth      LACP      Eth1/2(P)    Eth1/3(P)    
11    Po11(SU)    Eth      LACP      Eth1/1(P)  
```

- On N9K-2:

```
N9K-2# show port-channel summary
Flags:  D - Down        P - Up in port-channel (members)
        I - Individual  H - Hot-standby (LACP only)
        s - Suspended   r - Module-removed
        b - BFD Session Wait
        S - Switched    R - Routed
        U - Up (port-channel)
        p - Up in delay-lacp mode (member)
        M - Not in use. Min-links not met
--------------------------------------------------------------------------------
Group Port-       Type     Protocol  Member Ports
      Channel
--------------------------------------------------------------------------------
1     Po1(SU)     Eth      LACP      Eth1/2(P)    Eth1/3(P)    
11    Po11(SU)    Eth      LACP      Eth1/1(P)    
```



This concludes the **Section 4: vPC Verification** and end the entire **vPC Lab** practice.
