# 📦 TFTP Lab - Device Prep, Packet Capture, and UDP Analysis

## Lab Overview

This lab explores **TFTP (Trivial File Transfer Protocol)** and how it operates over **UDP**, using **Wireshark** for packet analysis. You'll simulate a TFTP file transfer between your host computer and a **Tiny-Core Linux VM** and examine how data flows at the **IP and UDP layers**.

---

## 🧰 Section 1 - Prepare Devices and Document Network Configuration

### Objective:

Set up the environment to allow TFTP transfers between the host and VM.

### Steps:

1. **Install Tftpd64** (from the "Related Information → Software" section on FOL).
2. **Launch VMware Workstation** and start the **INFO-6070-TinyCore VM**.
3. Identify network settings using:

   * On **Windows host**: `ipconfig`
   * On **Tiny-Core VM**: `ifconfig`, `route`, `cat /etc/network/interfaces`
4. Ensure both systems are connected through the **VMnet8 interface**.

---

## 📱 Section 2 - Connect to TFTP Server and Capture Data

### Objective:

Perform a TFTP file transfer and capture the packet data using Wireshark.

### Steps:

1. **Open Wireshark** and select the **VMnet8 interface**.
2. Set filter to:

   ```
   udp and host <VM_IP_address>
   ```
3. Launch **Tftpd64**, switch to the **TFTP Client** tab.
4. Configure:

   * **Host**: `<Tiny-Core VM IP>`
   * **Port**: `69`
   * **Local File**: Path to the file you want to send
   * **Remote File**: `TFTProot/123.jpg`
   * **Block Size**: Leave default
5. Click **Put** to start transfer.
6. Return to Wireshark, stop the capture and save it.

---

## 🔍 Section 3 - Analyze the Captured Data

### Question Summaries:

* **Q3: IP Protocol Number for UDP**

  * **Answer:** UDP uses **Protocol Number 17** in the IP header.

* **Q4: Screenshot Requirement**

  * Provide a screenshot showing **Protocol: UDP (17)** in the IP packet details.

* **Q5: UDP Header Fields**

  * **Answer:** 4 fields — Source Port, Destination Port, Length, Checksum.

* **Q6: UDP Header Details**

  | Field                           | Answer                        |
  | ------------------------------- | ----------------------------- |
  | UDP length field describes      | Header + Payload size         |
  | Size of UDP Header              | 8 bytes                       |
  | Size of Payload                 | 39 bytes (example)            |
  | Max UDP Payload Size (Standard) | 65,535 - 8 = **65,527 bytes** |
  | Well-known TFTP port            | **69**                        |

* **Q7: Screenshot Requirement**

  * Provide a screenshot showing **UDP Header** fields and lengths.

* **Q8: Acknowledgements in Info Column**

  * **Answer:** These are **TFTP Acknowledgements**, not from the UDP layer (since UDP is connectionless).

* **Q9: UDP Port Relationships**

  * **Client sends to port 69** from a random high-numbered port.
  * **Server replies from a new port** back to the client's original source port.

---

## ⚙️ Section 4 - UDP Transfer with Modified Block Size

### Objective:

Evaluate how increasing TFTP block size affects the transfer.

### Steps:

1. Start new Wireshark capture with:

   ```
   udp and host <VM_IP_address>
   ```
2. In TFTPd64:

   * Change **Block Size** to **32768**.
   * Repeat the file transfer.
3. Save the new Wireshark capture.

### Questions:

* **Q11: Does increased block size affect the transfer?**

  * **Answer:** Yes

* **Q12: Differences at IP Layer?**

  * **Answer:** IP packets are **fragmented** due to the larger payload size.

---

## 📌 Summary

Through this lab, you:

* Configured a TFTP client
* Transferred files using UDP
* Captured and analyzed real packet data
* Explored key UDP properties and port behavior
* Understood how TFTP leverages UDP and handles large data using fragmentation


