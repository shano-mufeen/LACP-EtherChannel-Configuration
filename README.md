# 🔗 EtherChannel Configuration Using LACP

## 📌 Project Overview

This project demonstrates the configuration and implementation of **EtherChannel** using **Link Aggregation Control Protocol (LACP)** in **Cisco Packet Tracer**.

The network consists of **five Cisco switches** connected using multiple physical links. Instead of treating each physical connection as a separate link, multiple physical links are combined together to form a single logical connection called an **EtherChannel**.

The project uses **LACP (Link Aggregation Control Protocol)** to dynamically negotiate and establish EtherChannel between the switches.

Unlike **PAgP**, which is a Cisco proprietary protocol, **LACP is an IEEE standard protocol** and can be used between devices from different vendors that support LACP.

The project also focuses on **LACP negotiation modes, channel groups, Port-Channel interfaces, trunk configuration, bandwidth utilization, redundancy, and EtherChannel verification**.

> 🎯 **Project Focus:** EtherChannel + LACP + Port-Channel + Trunking + Link Redundancy

# 📋 Case Study

A company requires reliable and higher-bandwidth connections between multiple network switches.

Multiple physical links are available between the switches. If these links are configured individually, **Spanning Tree Protocol (STP)** may place redundant links into a blocking state to prevent Layer 2 loops.

To make better use of the available physical links, the company wants to combine them into logical connections using **EtherChannel**.

For this project, **LACP** is used to negotiate and establish EtherChannel between the switches.

LACP is selected because it is an **IEEE-standard link aggregation protocol**, making it suitable for networks that may contain equipment from different vendors.

### Requirements

1. 🔗 Multiple physical links must be combined into EtherChannel
2. 📡 LACP must be used for EtherChannel negotiation
3. 🔀 Five Cisco switches must be connected using EtherChannel
4. 🌐 EtherChannel connections must operate as trunk links
5. 🛡️ Redundant physical links must provide better network reliability
6. 🚀 Multiple physical links should provide increased bandwidth
7. 🔄 LACP negotiation must use compatible `active` and `passive` modes
8. 🧪 EtherChannel operation must be verified using Cisco IOS commands

# 🎯 Project Objectives

By completing this project, you will learn how to:

* 🔹 Understand the purpose of EtherChannel
* 🔹 Understand how LACP works
* 🔹 Understand the difference between LACP and PAgP
* 🔹 Understand LACP negotiation modes
* 🔹 Configure LACP using `active` and `passive` modes
* 🔹 Create EtherChannel using channel groups
* 🔹 Configure Port-Channel interfaces
* 🔹 Configure EtherChannel as a trunk
* 🔹 Combine multiple physical interfaces into one logical link
* 🔹 Understand EtherChannel and STP interaction
* 🔹 Improve bandwidth utilization between switches
* 🔹 Provide link redundancy
* 🔹 Verify EtherChannel operation
* 🔹 Troubleshoot LACP and EtherChannel configuration issues

# 🏢 Network Design

The network contains **five Cisco switches** connected using multiple physical links.

The topology contains multiple EtherChannel connections, with each EtherChannel using **3 physical links**.

The physical links are bundled into logical Port-Channel interfaces using LACP.

```text
                         ┌─────────────────┐
                         │     Switch 1    │
                         │    Cisco 2960   │
                         └────────┬────────┘
                                  ║║║
                                  ║║║
                             3 Physical Links
                                  ║║║
                                  ▼
                         ┌─────────────────┐
                         │     Switch 2    │
                         │    Cisco 2960   │
                         └────────┬────────┘
                                  │
                 ┌────────────────┴────────────────┐
                 │                                 │
              ║║║                               ║║║
           3 Links                          3 Links
                 │                                 │
                 ▼                                 ▼
        ┌─────────────────┐              ┌─────────────────┐
        │     Switch 3    │              │     Switch 4    │
        │    Cisco 2960   │              │    Cisco 2960   │
        └─────────────────┘              └─────────────────┘

                         LACP EtherChannels
```

> 💡 The exact physical interface numbers depend on the interfaces selected in the Cisco Packet Tracer topology.

# 🔗 EtherChannel Concept

EtherChannel combines multiple physical interfaces into a **single logical interface**.

For example, three physical links can be bundled together:

```text
Physical Links

Fa0/1 ─────────────────────── Fa0/1
Fa0/2 ─────────────────────── Fa0/2
Fa0/3 ─────────────────────── Fa0/3
          │
          ▼
      EtherChannel
          │
          ▼
     Port-Channel
```

The switches treat these physical links as one logical connection.

This allows multiple physical links to participate in the EtherChannel while STP views the bundled connection as a single logical path.

# 📡 LACP — Link Aggregation Control Protocol

**LACP (Link Aggregation Control Protocol)** is an **IEEE-standard protocol** used to dynamically negotiate and establish EtherChannel.

LACP is defined by the IEEE and is not limited to Cisco devices.

This makes LACP different from **PAgP**, which is Cisco proprietary.

### PAgP vs LACP

| Feature        | PAgP                      | LACP                              |
| -------------- | ------------------------- | --------------------------------- |
| Full Form      | Port Aggregation Protocol | Link Aggregation Control Protocol |
| Standard       | Cisco Proprietary         | IEEE Standard                     |
| Vendor Support | Cisco                     | Multi-vendor                      |
| Modes          | `desirable`, `auto`       | `active`, `passive`               |
| EtherChannel   | ✅                         | ✅                                 |

# ⚙️ LACP Negotiation Modes

LACP uses two main negotiation modes:

| Mode      | Description                             |
| --------- | --------------------------------------- |
| `active`  | Actively sends LACP negotiation packets |
| `passive` | Waits for LACP negotiation packets      |

### LACP Compatibility

| Side A    | Side B    | Result                       |
| --------- | --------- | ---------------------------- |
| `active`  | `active`  | ✅ EtherChannel forms         |
| `active`  | `passive` | ✅ EtherChannel forms         |
| `passive` | `active`  | ✅ EtherChannel forms         |
| `passive` | `passive` | ❌ EtherChannel does not form |

> 💡 **Key Point:** At least one side must be configured as `active` for LACP negotiation to establish the EtherChannel.

# 🌐 EtherChannel and Trunking

The EtherChannel connections in this project are configured as **trunk links**.

After creating the channel group, the logical Port-Channel interface is configured as a trunk.

```cisco
interface port-channel 1
switchport mode trunk
```

The same configuration is applied to the other Port-Channel interfaces.

This allows multiple VLANs to travel across the logical EtherChannel connection.

# 🔄 STP Before EtherChannel

Before EtherChannel is configured, multiple physical connections between switches can create a Layer 2 loop.

STP may therefore place redundant links into a blocking state.

```text
Switch 1                         Switch 2

Fa0/1  🟢──────────────────────── Fa0/1
Fa0/2  🟠──────────────────────── Fa0/2
Fa0/3  🟠──────────────────────── Fa0/3

        STP blocks redundant links
```

Although the additional links provide physical redundancy, they may not all forward traffic independently.

EtherChannel solves this by combining the physical links into one logical connection.

# 🚀 STP After EtherChannel

After EtherChannel is configured, the physical links are bundled into a single logical Port-Channel.

```text
Switch 1                         Switch 2

Fa0/1  ══════════════════════════
Fa0/2  ══════════════════════════
Fa0/3  ══════════════════════════
             │
             ▼
       Port-Channel 1
```

STP sees the EtherChannel as one logical link rather than three independent links.

The physical interfaces within the EtherChannel can therefore participate in forwarding as members of the logical channel.

# ⚙️ Configuration Process

The configuration is divided into the following steps:

1. 🖥️ Build the network topology
2. 🔌 Identify the physical interfaces
3. 🌐 Identify the switch-to-switch trunk links
4. 🔗 Configure LACP channel groups
5. ⚙️ Configure LACP negotiation modes
6. 🔀 Configure Port-Channel interfaces
7. 🌐 Configure Port-Channels as trunk links
8. 🧪 Verify EtherChannel operation

# 1️⃣ Build the Network Topology

Create the five-switch topology in Cisco Packet Tracer.

The topology contains:

* 🔹 5 Cisco switches
* 🔹 Multiple switch-to-switch connections
* 🔹 3 physical links per EtherChannel
* 🔹 Multiple LACP channel groups
* 🔹 Logical Port-Channel interfaces

Before starting the configuration, make sure the required interfaces are available and operational.

For the Cisco 3650 multilayer switch used in the topology, ensure the required power supply/module configuration is present so that the interfaces can become operational.

# 2️⃣ Identify the Physical Interfaces

Identify the interfaces connecting each switch.

For example:

```text
Fa0/1
Fa0/2
Fa0/3
```

These three interfaces can be combined into:

```text
Channel Group 1
        ↓
Port-Channel 1
```

Another set of interfaces can be assigned to:

```text
Channel Group 2
        ↓
Port-Channel 2
```

Each physical interface participating in the same EtherChannel must be configured consistently.

# 3️⃣ Configure LACP — Channel Group 1

For the first EtherChannel, three physical FastEthernet interfaces are selected.

```cisco
enable
configure terminal

interface range fa0/1 - 3
channel-group 1 mode active
exit
```

Here:

```text
channel-group 1
        ↓
Creates Channel Group 1

mode active
        ↓
Enables active LACP negotiation
```

The neighboring switch can use either `active` or `passive` mode.

# 4️⃣ Configure Port-Channel 1

After creating Channel Group 1, configure the logical Port-Channel interface.

```cisco
interface port-channel 1
switchport mode trunk
exit
```

### Configuration Result

```text
Fa0/1 ─┐
Fa0/2 ─┼── Channel Group 1 ── Port-Channel 1
Fa0/3 ─┘
              │
              ▼
            Trunk
```

# 5️⃣ Configure the Neighboring Switch

The neighboring switch uses the same **channel group number** and a compatible LACP mode.

```cisco
enable
configure terminal

interface range fa0/4 - 6
channel-group 1 mode passive
exit
```

Because the first switch is using:

```text
active
```

and the neighboring switch is using:

```text
passive
```

the LACP EtherChannel can form.

Configure the Port-Channel:

```cisco
interface port-channel 1
switchport mode trunk
exit
```

# 6️⃣ Configure Channel Group 2

The next EtherChannel uses **Channel Group 2**.

Example:

```cisco
interface range fa0/1 - 3
channel-group 2 mode passive
exit
```

The neighboring switch should use a compatible mode such as:

```cisco
channel-group 2 mode active
```

### LACP Relationship

```text
Switch A                  Switch B

Passive  ◄──────────────► Active

          LACP
           ↓
      EtherChannel
           ↓
     Port-Channel 2
```

# 7️⃣ Configure Port-Channel 2

Configure the logical interface:

```cisco
interface port-channel 2
switchport mode trunk
exit
```

The three physical interfaces are now represented by the logical Port-Channel 2.

# 8️⃣ Configure Additional EtherChannels

The same process is repeated for the remaining switch connections.

For example, Channel Group 3:

```cisco
interface range gigabitEthernet 1/0/1 - 3
channel-group 3 mode passive
exit
```

Configure Port-Channel 3:

```cisco
interface port-channel 3
switchport mode trunk
exit
```

# 9️⃣ Configure Channel Group 3 on the Neighboring Switch

On the neighboring switch, configure the corresponding physical interfaces using the same channel group number.

```cisco
interface range fa0/1 - 3
channel-group 3 mode active
exit
```

Then configure the Port-Channel:

```cisco
interface port-channel 3
switchport mode trunk
exit
```

# 🔟 Configure Channel Group 4

Another EtherChannel can be configured using Channel Group 4.

```cisco
interface range fa0/4 - 6
channel-group 4 mode active
exit
```

Configure the logical interface:

```cisco
interface port-channel 4
switchport mode trunk
exit
```

The neighboring switch should use a compatible LACP mode:

```cisco
interface range fa0/4 - 6
channel-group 4 mode passive
exit
```

Then:

```cisco
interface port-channel 4
switchport mode trunk
exit
```

# 🔢 Channel Group Configuration Rule

Both sides of an EtherChannel connection should use the **same channel group number** for the corresponding logical EtherChannel.

Example:

```text
Switch A                         Switch B

Channel Group 1  ═══════════════ Channel Group 1
      ↓                                  ↓
Port-Channel 1                    Port-Channel 1
```

For another connection:

```text
Switch A                         Switch B

Channel Group 2  ═══════════════ Channel Group 2
      ↓                                  ↓
Port-Channel 2                    Port-Channel 2
```

> 💡 The channel group number identifies which physical interfaces belong to the same EtherChannel.

# 📊 LACP Channel Mapping

| EtherChannel   | Physical Links | Channel Group | Port-Channel | LACP Modes       |
| -------------- | -------------: | ------------: | -----------: | ---------------- |
| EtherChannel 1 |              3 |             1 |          Po1 | Active ↔ Passive |
| EtherChannel 2 |              3 |             2 |          Po2 | Active ↔ Passive |
| EtherChannel 3 |              3 |             3 |          Po3 | Active ↔ Passive |
| EtherChannel 4 |              3 |             4 |          Po4 | Active ↔ Active  |

> 📝 The exact interface numbers depend on the interfaces selected in the Packet Tracer topology.

# 🧪 Verification

After completing the configuration, verify the EtherChannel operation using Cisco IOS commands.

## 🔍 Verify EtherChannel Summary

```cisco
show etherchannel summary
```

This command displays:

* 🔢 EtherChannel group number
* 🔀 Port-Channel number
* 📡 Protocol
* 🔌 Member interfaces
* 🟢 Channel status

The protocol should show:

```text
LACP
```

Example:

```text
Group  Port-channel  Protocol
-----  -------------  --------
1      Po1(SU)        LACP
2      Po2(SU)        LACP
3      Po3(SU)        LACP
4      Po4(SU)        LACP
```

## 🔍 Verify Trunk Configuration

```cisco
show interfaces trunk
```

This command verifies that the Port-Channel interfaces are operating as trunk links.

## 🔍 Verify Port-Channel Interface

```cisco
show interfaces port-channel 1
```

For additional EtherChannels:

```cisco
show interfaces port-channel 2
show interfaces port-channel 3
show interfaces port-channel 4
```

## 🔍 Verify Interface Status

```cisco
show interfaces status
```

This can be used to check the status of the physical interfaces participating in EtherChannel.

## 🔍 Verify Running Configuration

```cisco
show running-config
```

This allows the configured LACP, channel-group, Port-Channel, and trunk settings to be reviewed.

# 🛠️ Troubleshooting

If the EtherChannel does not form correctly, check the following:

### 1. 📡 Check LACP Modes

Make sure at least one side is configured as `active`.

```text
Active + Active       → ✅
Active + Passive      → ✅
Passive + Active      → ✅
Passive + Passive     → ❌
```

### 2. 🔢 Check Channel Group Numbers

The corresponding interfaces on both switches should use the same channel group number.

```cisco
channel-group 1 mode active
```

### 3. 🔌 Check Physical Interfaces

Make sure the correct physical interfaces are included in the channel group.

```cisco
interface range fa0/1 - 3
```

### 4. 🌐 Check Trunk Configuration

Verify that the Port-Channel is configured as a trunk.

```cisco
interface port-channel 1
switchport mode trunk
```

### 5. 🧪 Check EtherChannel Status

Use:

```cisco
show etherchannel summary
```

Confirm that the physical interfaces are successfully bundled into the Port-Channel.

### 6. ⚙️ Check Interface Consistency

Interfaces participating in the same EtherChannel should have compatible configurations.

Check:

* 🔹 Speed
* 🔹 Duplex
* 🔹 Switchport mode
* 🔹 VLAN configuration
* 🔹 Channel group
* 🔹 LACP mode

# 📈 Benefits Achieved

After implementing LACP EtherChannel, the network provides:

| Feature          | Before EtherChannel         | After EtherChannel                   |
| ---------------- | --------------------------- | ------------------------------------ |
| Physical links   | Separate                    | Bundled                              |
| Logical links    | Multiple                    | Single Port-Channel                  |
| STP handling     | Individual links            | Logical EtherChannel                 |
| Link utilization | Some links may be blocked   | Links participate as an EtherChannel |
| Redundancy       | Individual physical links   | Bundled link redundancy              |
| Bandwidth        | Limited by active path      | Increased through link aggregation   |
| Protocol         | No EtherChannel negotiation | LACP negotiation                     |
| Management       | Multiple interfaces         | Logical Port-Channel                 |

# 🧠 Key Concepts

### 🔗 EtherChannel

Combines multiple physical interfaces into a single logical connection.

### 📡 LACP

An **IEEE-standard protocol** used to dynamically negotiate EtherChannel.

### 🟢 Active

Actively sends LACP negotiation packets.

### 🟡 Passive

Waits for LACP negotiation packets from the neighboring device.

### 🔀 Channel Group

Groups physical interfaces together to create an EtherChannel.

### 🌐 Port-Channel

The logical interface created by EtherChannel.

### 🚪 Trunk

Allows multiple VLANs to travel across the EtherChannel connection.

### 🛡️ STP

Prevents Layer 2 loops and treats an EtherChannel as a single logical path.

# 📝 Important Commands

```cisco
enable
configure terminal

interface range fa0/1 - 3
channel-group 1 mode active
exit

interface port-channel 1
switchport mode trunk
exit

show etherchannel summary
show interfaces trunk
show interfaces port-channel 1
show interfaces status
show running-config
```

# 🔄 PAgP vs LACP Quick Revision

```text
                 EtherChannel
                      │
             ┌────────┴────────┐
             │                 │
            PAgP              LACP
             │                 │
       Cisco Proprietary    IEEE Standard
             │                 │
      ┌──────┴──────┐   ┌──────┴──────┐
      │             │   │             │
  Desirable       Auto Active       Passive
      │             │   │             │
      └──────┬──────┘   └──────┬──────┘
             │                 │
       EtherChannel        EtherChannel
```

### PAgP

```text
Desirable + Desirable → ✅
Desirable + Auto      → ✅
Auto + Auto           → ❌
```

### LACP

```text
Active + Active       → ✅
Active + Passive      → ✅
Passive + Active      → ✅
Passive + Passive     → ❌
```

# 🎯 Project Outcome

The project demonstrates how multiple physical switch-to-switch links can be combined into logical EtherChannel connections using **LACP**.

Multiple channel groups were configured across the five-switch topology, with each EtherChannel using multiple physical interfaces.

```text
Multiple Physical Links
          ↓
     Channel Group
          ↓
    LACP Negotiation
          ↓
      Port-Channel
          ↓
       Trunk Link
          ↓
Improved Bandwidth
      + Redundancy
```

The configuration was verified using Cisco IOS commands, confirming the operation of **LACP, Port-Channels, and trunk links**.

# 🚀 Conclusion

This project provides practical experience in configuring **EtherChannel using LACP** in Cisco Packet Tracer.

By combining multiple physical interfaces into logical Port-Channels, the network can make better use of available links while maintaining redundancy.

The project also demonstrates the relationship between **EtherChannel, LACP, STP, channel groups, Port-Channels, and trunking**.

Most importantly, the project highlights the difference between **PAgP and LACP**:

* 🔵 **PAgP** → Cisco proprietary
* 🟢 **LACP** → IEEE standard
* 🔵 **PAgP modes** → `desirable` / `auto`
* 🟢 **LACP modes** → `active` / `passive`

# 📚 Technologies & Tools

* 🖥️ Cisco Packet Tracer
* 🔀 Cisco Switching
* 🔗 EtherChannel
* 🟢 LACP
* 🌐 VLAN Trunking
* 🛡️ Spanning Tree Protocol
* 💻 Cisco IOS CLI

# 🏷️ Tags

`#Networking` `#Cisco` `#CCNA` `#EtherChannel` `#LACP` `#LinkAggregation` `#Switching` `#PortChannel` `#Trunking` `#STP` `#CiscoPacketTracer`
