
# VMware Virtual Lab Setup Documentation

## Step 1: Installed Virtual Machines

I installed the following VMs. I will not be providing the VM files themselves but can share the website where I downloaded them from:

- `kali-linux-2025`
- `Windows 10 Operating System`
- `Windows Server 2016`
- `Ubuntu Web Server`
- `Metasploitable-2`

![Downloaded VMs](path/to/downloaded-files-image.png)

---

## Step 2: Verifying Checksums

As a security-aware individual, always confirm checksums with the software vendor.

I ran the following command in **Command Prompt** to confirm checksums for the VMs:

```powershell
Get-FileHash -Algorithm SHA1 –Path ".\C:\*.*"
```

![Checksum Confirmation](path/to/checksum-image.png)

**Vendor Checksum File Example:**

```
metasploitable-linux-2.0.0.zip          3BD77207F74B7031E50F7CD83539BD401B1284FF  
ubuntu-18.04-live-server-amd64.iso     0B3490DE9839C3918E35F01AA8A05C9AE286FC94  
Win10.7z                                481378AF6CE94A16CBED1D70C49BD9E00AF1E5B2  
win2016x64.7z                           18608362F51FDC9A20F70534A8F0108BDD1CDDD4
```

---

## Step 3: Kali Linux VM Configuration

- Open Kali on VMware Workstation and enter credentials.
- Update packages:

```bash
sudo apt-get update
sudo apt-get upgrade
```

- Change hostname:

```bash
sudo nano /etc/hostname
```
Replace `kali` with your username.

- Update `/etc/hosts`:

```bash
sudo nano /etc/hosts
```

Add:
```
127.0.0.1  username
127.0.0.1  username-kali
```

Add entries for web servers:
```
10.0.0.200  ubuntuwebserver
10.0.0.201  windowserver
10.0.0.202  windows10
```

---

## Step 4: Creating a LAN Segment in VMware

1. In VMware: `VM > Settings > Add New Network Adapter`
2. Select `LAN Segment`, name it, and set static IP configuration.

- Modify Kali’s network interface:

```bash
sudo nano /etc/network/interfaces
```

Add:
```
auto eth1
iface eth1 inet static
address 10.0.0.99
netmask 255.255.255.0
```

Reboot the VM.

![LAN Segment Config](path/to/lan-segment-image.png)

---

## Preparing Windows 10 VM

- Extract and load into VMware Workstation.
- Change network adapter to LAN Segment.
- Modify disk space.
- Power on the VM.
- Change computer name.
- Disable Windows Firewall.
- Assign static IP:

```
IP Address: 10.0.0.100  
Subnet Mask: 255.255.255.0
```

- Ping Kali Linux from Windows CMD.

```cmd
ping 10.0.0.99
```

![Ping from Windows](path/to/ping-kali-image.png)

---

## Preparing Windows Server 2016 VM

- Load VM into VMware.
- Change network adapter to LAN Segment.
- Modify disk space.
- Power on the VM.
- Change computer name.
- Disable Firewall.
- Assign static IP:

```
IP Address: 10.0.0.201  
Subnet Mask: 255.255.255.0
```

- Ping Windows Server from Kali:

```bash
ping 10.0.0.201
```

![Ping Windows Server](path/to/ping-winserver-image.png)

---

## Setting up Metasploitable Server

- Load VM into VMware.
- Change network adapter to LAN Segment.
- Login with default credentials.
- Change hostname.
- Edit network interfaces:

```bash
sudo nano /etc/network/interfaces
```

Add:
```
address 10.0.0.202
netmask 255.255.255.0 
network 10.0.0.0 
broadcast 10.0.0.255
```

- Reboot and ping from Kali:

```bash
ping 10.0.0.202
```

---

## Setting up Ubuntu Web Server

- Load and configure Ubuntu Web Server.
- Change hostname.
- Add another network adapter.
- Reboot VM.
- Navigate to netplan:

```bash
cd /etc/netplan
ls -al
```

- Copy and modify the YAML config file using a text editor.

![Netplan Config](path/to/netplan-image.png)

- Save and apply changes:

```bash
sudo netplan apply
```

---

## Installing Mutillidae on Ubuntu Web Server

- Set location:

```bash
sudo dpkg-reconfigure tzdata
```

Select `America/Toronto`.

- Sync time:

```bash
sudo date -s "$(wget -qSO- --max-redirect=0 google.com 2>&1 | grep Date: | cut -d' ' -f5-8)Z"
```

- Restart VM:

```bash
sudo shutdown -r now
```

- Log in with `FOLusername` and password: `Ubuntu1`

- Update repository:

```bash
sudo apt-get update
```

- Update hosts file:

```bash
sudo nano /etc/hosts
```

Add:
```
127.0.0.1  FOLusername-uws
```

- Install Apache:

```bash
sudo apt-get install apache2 apache2-utils
```

- Install MySQL:

```bash
sudo apt-get install mysql-server
```

- Configure MySQL:

```bash
sudo mysql -u root
```

In MySQL shell:
```sql
use mysql;
update user set authentication_string=PASSWORD('') where user='root';
update user set plugin='mysql_native_password' where user='root';
flush privileges;
quit;
```

- Restart MySQL:

```bash
sudo service mysql restart
```

- Install unzip:

```bash
sudo apt-get install unzip
```

- Download Mutillidae script:

```bash
cd /var/www
sudo wget http://transpirenetworks.com/mutillidae_setup.sh
```

> Alternative: Use `mutillidae_setup.7z` with password `info6076`.

- Execute the script:

```bash
sudo bash mutillidae_setup.sh
```

- Unzip the archive:

```bash
sudo unzip LATEST-mutillidae-2.6.62.zip
```

- Open browser in Kali and navigate to:

```
http://FOLusername-uws/mutillidae
```

> Click on **Setup / Reset Database** if needed.

![Mutillidae Success](path/to/mutillidae-image.png)

---

Let me know if you'd like placeholders replaced with actual images.
