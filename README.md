# **File Sharing and Collaboration System Using Samba**

## **Description**  
This project demonstrates setting up a file-sharing system using Samba on Linux. It enables seamless file sharing between Linux and Windows, with user-based access control and guest access options. The guide covers creating shared directories, configuring Samba, and testing the setup.

---

## **Prerequisites**  
Before starting, ensure the following:  
- A Linux system (tested on Ubuntu/Debian).  
- Basic command-line knowledge.  
- Administrative privileges (root or sudo access).  
- Installed packages: Samba, Nano (or another text editor).

---

## **Setup Instructions**  

### **1. Update Your System**  
Ensure your system is up-to-date for stable performance and security patches:  
```bash
sudo apt update && sudo apt upgrade -y
```
## **Install Samba**
### **Install the Samba package:**
```bash
sudo apt install samba -y
```
### **Verify the installation:**
```bash
samba --version
```

## **Create a Shared Directory**
### **Create a directory to serve as the shared folder:**
```bash
sudo mkdir -p /srv/shared
```
### **Set permissions to allow access:**
```bash
sudo chown nobody:nogroup /srv/shared
sudo chmod 777 /srv/shared
```
## **Configure Samba**
### **Back up the default Samba configuration:**
```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```
### **Open the Samba configuration file for editing:**
```bash
sudo nano /etc/samba/smb.conf
```
### **Add the following under the [global] section:**
```ini
[Shared]
path = /srv/shared
browsable = yes
writable = yes
guest ok = yes
read only = no
```
### **Save and exit (Ctrl + O, Enter, Ctrl + X).**
### **Restart the Samba service to apply the changes:**
```bash
sudo systemctl restart smbd
```
## **Configure User-Based Access (Optional)**
### **Add a Samba user:**
```bash
sudo smbpasswd -a <username>
```
### **Replace <username> with the desired username.**
### **Restrict the shared folder to this user:**
```bash
sudo chown <username>:<username> /srv/shared
sudo chmod 770 /srv/shared
```
### **Update the [Shared] section in smb.conf:**
```ini
valid users = <username>
```
### **Restart Samba:**
```bash
sudo systemctl restart smbd
```
## **Testing Instructions**
## **Verify Samba Setup**
### **List available shares:**
```bash
smbclient -L localhost -U <username>
```
### **Replace <username> with the Samba username created earlier.**
### **2. Test File Access**

- **From Linux:**  
  Use the `smbclient` tool:  
  ```bash
  smbclient //<server-ip>/Shared -U <username>
  ```
  ### **Replace <server-ip> with the IP address of the server.**
  - **From Windows:**  
  1. Open **File Explorer**.  
  2. Enter `\\<server-ip>\Shared` in the address bar.  
  3. Log in using the Samba username and password.
## **Future Improvements**  

1. **Encryption:**  
   - Enable SMB3 protocol for encrypted file transfers.  

2. **Multi-User Support:**  
   - Add multiple user accounts with specific permissions.  

3. **Logging and Monitoring:**  
   - Enable detailed logs to track access and errors.  

4. **Automated Setup Script:**  
   - Create a Bash script to automate the entire setup process.  

5. **Advanced Authentication:**  
   - Integrate Samba with an LDAP server for centralized user management.
