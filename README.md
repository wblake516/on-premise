<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# **On-Premises Active Directory Deployed in the Cloud (Azure)**
This guide provides a step-by-step walkthrough for implementing on-premises **Active Directory** within **Azure Virtual Machines**.

---

## **Environments and Technologies Used**
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Protocol (RDP)
- Active Directory Domain Services (AD DS)
- PowerShell

---

## **Operating Systems Used**
- Windows Server 2022
- Windows 10 (21H2)

---

## **Deployment and Configuration Steps**

### **Step 1: Create and Configure Virtual Machines**
<p>
<img src="https://i.imgur.com/FiST5JT.png" alt="Virtual Machines Setup"/>
</p>
- Create two virtual machines:
  - **DC-1** running Windows Server 2022 (Domain Controller)
  - **Client1** running Windows 10
- Ensure both VMs are on the same **virtual network (VNet)**.
- Change **DC-1's IP address** from **dynamic to static** to allow domain connectivity.

---

### **Step 2: Disable Firewall on the Domain Controller**
<p>
<img src="https://i.imgur.com/wIfUJdl.png" alt="Disable Firewall"/>
</p>
- RDP into **DC-1**.
- Open **Windows Defender Firewall** and disable it for all profiles (Domain, Private, Public).

---

### **Step 3: Configure DNS Settings on Client1**
<p>
<img src="https://i.imgur.com/ER2Lw3V.png" alt="DNS Configuration"/>
</p>
- Change **Client1’s DNS settings** to point to **DC-1’s static private IP**.
- Restart both VMs to apply DNS changes.

---

### **Step 4: Verify Connectivity Between Client1 and DC-1**
<p>
<img src="https://i.imgur.com/0zFPyiA.png" alt="Ping DC-1 from Client1"/>
<img src="https://i.imgur.com/xm02kle.png" alt="Confirm DNS Configuration"/>
</p>
- Ping **DC-1** from **Client1** via **PowerShell** to confirm connectivity.
- Run `ipconfig /all` to verify **DC-1 is the DNS server**.

---

### **Step 5: Install Active Directory Domain Services (AD DS)**
<p>
<img src="https://i.imgur.com/oBYt50h.png" alt="Install AD DS"/>
</p>
- Install **Active Directory Domain Services (AD DS)** via **Server Manager** on **DC-1**.

---

### **Step 6: Promote DC-1 to a Domain Controller**
<p>
<img src="https://i.imgur.com/1UY0lrq.png" alt="Promote DC-1 to Domain Controller"/>
</p>
- Promote **DC-1** to a **Domain Controller** and create a **new forest** with the domain name `mydomain.com`.

---

### **Step 7: Create Organizational Units (OUs)**
<p>
<img src="https://i.imgur.com/lRyBbvA.png" alt="Create Organizational Units"/>
</p>
- Open **Active Directory Users and Computers (ADUC)**.
- Create **_EMPLOYEES** and **_ADMINS** organizational units.

---

### **Step 8: Create a Domain Administrator User**
<p>
<img src="https://i.imgur.com/lj5a6an.png" alt="Create Admin User"/>
</p>
- Inside the **Admins OU**, create a user named **Ken Doe** with the username `ken_admin`.

---

### **Step 9: Assign Domain Admin Privileges**
<p>
<img src="https://i.imgur.com/lHZsq7U.png" alt="Assign Domain Admin Privileges"/>
</p>
- Right-click **Ken Doe**, go to **Properties > Member Of**.
- Add **Domain Admins** group to grant admin privileges.

---

### **Step 10: Log in as Domain Admin**
<p>
<img src="https://i.imgur.com/x1tbaQ1.png" alt="Log in as Domain Admin"/>
</p>
- Log out of **DC-1**.
- Reconnect using **RDP** with credentials `mydomain.com\ken_admin`.

---

### **Step 11: Join Client1 to the Domain**
<p>
<img src="https://i.imgur.com/sWL89qz.png" alt="Join Client1 to the Domain"/>
</p>
- RDP into **Client1**.
- Navigate to **System > Rename this PC (Advanced) > Change > Domain**.
- Enter `mydomain.com` and restart the VM.

---

### **Step 12: Create a _CLIENTS Organizational Unit**
<p>
<img src="https://i.imgur.com/wsOjP9T.png" alt="Create _CLIENTS Organizational Unit"/>
</p>
- In **ADUC**, create a new **_CLIENTS OU** under `mydomain.com`.

---

### **Step 13: Enable Remote Desktop for Domain Users**
<p>
<img src="https://i.imgur.com/HgTs346.png" alt="Enable Remote Desktop for Domain Users"/>
</p>
- On **Client1**, allow **Domain Users** to access **Remote Desktop**.

---

### **Step 14: Automate User Creation with PowerShell**
<p>
<img src="https://i.imgur.com/DoCgqOP.png" alt="PowerShell User Creation Script"/>
</p>
- Open **PowerShell ISE** on **DC-1** as **ken_admin**.
- Download and run a **user creation script**:
  
  [PowerShell Script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

---

### **Step 15: Verify User Accounts in Active Directory**
<p>
<img src="https://i.imgur.com/Ca3ShWL.png" alt="Verify Users in ADUC"/>
</p>
- Open **ADUC** and confirm that the new user accounts exist in **_EMPLOYEES OU**.

---

### **Step 16: Log in to Client1 with a New User**
<p>
<img src="https://i.imgur.com/PWd6tW8.png" alt="Log in with New User"/>
</p>
- Log into **Client1** using one of the newly created **Active Directory accounts**.
- Verify the login via **PowerShell**.

---

## **Conclusion**
By following these steps, you have successfully deployed and configured **Active Directory** on **Azure Virtual Machines**. This setup enables centralized domain management, user authentication, and enhanced security controls within your cloud infrastructure.
