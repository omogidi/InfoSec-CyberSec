
# Lab Report: Protocol Analysis Using Wireshark

## Objective

To perform protocol analysis using a packet analyzer program (Wireshark) and understand basic network traffic such as ping and traceroute.

---

## Analyzing Ping Traffic

1. Get your IP and gateway details by running in CMD:

```cmd
ipconfig /all
```

2. Open Wireshark.
3. Click on the network interface to capture from (e.g., WiFi).
4. In the **"Use a filter"** field, enter the following:

```
host x.x.x.x and icmp
```

Replace `x.x.x.x` with your **gateway IP address**.
5. Press **Enter** to start capturing.
6. Open CMD on your host and run:

```cmd
ping x.x.x.x
```

You will begin to see captured traffic in Wireshark.

*...Add Image...*

### Explanation:

- The **Source** and **Destination** IP addresses represent the endpoints of the ping.
- The **Protocol** used is **ICMP** (Internet Control Message Protocol).
- The **Info** column shows:
  - `Echo Request` (Type 8, Code 0)
  - `Echo Reply` (Type 0, Code 0)
- Look at the ICMP section in the detailed packet view:
  - **Total length**: 74 bytes
  - **Header length**: 42 bytes
  - **Data portion**: 32 bytes

*...Add Image...*

---

## Analyzing Traceroute (tracert) Traffic

1. Close the current capture in Wireshark (**File > Close**).
2. Click your Ethernet or Wireless interface (single click).
3. In the filter field, enter:

```
host x.x.x.x and icmp
```

Replace `x.x.x.x` with the **target IP address**.
4. Press **Enter** to apply the filter.
5. In CMD, run:

```cmd
tracert www.example.com
```

6. Allow the command to complete.
7. Stop the Wireshark capture.

*...Add Image...*

### Analysis:

- Observe the TTL (Time-To-Live) values decrement as the packets move through routers.
- Each router that decreases the TTL responds with an **ICMP Time Exceeded** message.
- Final destination responds with **ICMP Echo Reply**.
- You can analyze the **hop count**, **IP addresses of each hop**, and timing.

---
