# KB-009: 🏗️ Building a Homelab in Hyper-V: The Step-by-Step Guide

This guide covers the complete, scratch-built setup of an Enterprise IT Support homelab using Microsoft Hyper-V. It includes the rationale behind every configuration choice to ensure you understand *why* we build it this way.

---

## Part 1: Hyper-V Virtual Networking (The "Default Switch" vs "Internal")

Hyper-V manages how your virtual machines talk to each other and the internet using Virtual Switches.

**The Choice:** We use the built-in **Default Switch**.
**The "Why":** The Default Switch acts like a home router with Network Address Translation (NAT). It gives your VMs internet access while keeping them isolated in their own private subnet, invisible to the rest of your physical home network. This keeps your lab secure and portable.

---

## Part 2: Building DC01 (The Domain Controller)

### Step 2.1: Create the VM
1. Open Hyper-V Manager -> **New** -> **Virtual Machine**.
2. **Name:** `DC01`
3. **Generation:** Choose **Generation 2** (Supports modern UEFI firmware and security features).
4. **Memory:** `2048 MB` (2GB). *Ensure "Use Dynamic Memory" is CHECKED so it only uses RAM when needed.*
5. **Networking:** Select **Default Switch**.
6. **Hard Drive:** `40 GB` is plenty for a Domain Controller.
7. **Installation Options:** Point it to your Windows Server 2022 ISO.

### Step 2.2: Install Windows Server
1. Start the VM and press any key to boot from the CD/DVD.
2. Choose **Windows Server 2022 Standard Evaluation (Desktop Experience)**. 
   > *Why? Without "Desktop Experience," you get the Core version, which has no GUI. We want the GUI for learning.*
3. Choose **Custom: Install Windows only (advanced)** and click Next.

### Step 2.3: Configure the Network (The Tricky Part)
A Domain Controller *must* have a static IP address. Because the Hyper-V Default Switch dynamically assigns IPs, we have to lock one in.
1. Once logged into `DC01`, open Command Prompt and type `ipconfig /all`.
   > *Why? This command displays your current network configuration. By running this before setting a static IP, we can copy the exact valid IP address, Subnet Mask, and Gateway that Hyper-V just handed us. This guarantees we stay in the correct virtual "neighborhood."*
2. Note down the **IPv4 Address**, **Subnet Mask**, and **Default Gateway**.
3. Open `ncpa.cpl` (Network Connections) -> Right-click adapter -> **Properties** -> **IPv4** -> **Properties**.
4. Check **Use the following IP address** and enter the exact IP, Subnet, and Gateway you just noted down.
5. For the **Preferred DNS Server**, enter `127.0.0.1` (This tells the server to look at itself for DNS, which is required for a Domain Controller).

### Step 2.4: Promote to Domain Controller
1. Open **Server Manager** -> **Add roles and features**.
2. Check **Active Directory Domain Services (AD DS)** and **DNS Server**. Install them.
3. Click the yellow flag at the top of Server Manager -> **Promote this server to a domain controller**.
4. Select **Add a new forest** and enter your root domain: `helpdesk.lab`.
5. Enter a Directory Services Restore Mode (DSRM) password (make it the same as your admin password so you don't forget it).
6. Click Next until you reach **Install**. The server will reboot.

### Step 2.5: Fix the "No Internet" Issue (DNS Forwarders)
Because `DC01` now looks at itself for DNS, it doesn't know how to find external websites.
1. Open **DNS Manager** from the Start Menu.
2. Right-click `DC01` on the left -> **Properties** -> **Forwarders** tab.
3. Click **Edit...** and add `8.8.8.8` (Google's Public DNS).
   > *Why? Now, if a client asks for "google.com", DC01 will forward the request to Google instead of failing.*
4. **Verify Internet Routing:** Open Command Prompt and type `tracert 8.8.8.8`.
   > *Why? Trace Route (`tracert`) shows the exact path your data takes hop-by-hop. If you see it successfully hopping from your Default Gateway out to the internet, it proves your internet routing is working perfectly.*

---

## Part 3: Building CLIENT01 (The Workstation)

### Step 3.1: Create the VM
1. **Name:** `CLIENT01`
2. **Generation:** **Generation 2**.
3. **Memory:** `4096 MB` (4GB). *Check "Use Dynamic Memory".*
4. **Networking:** **Default Switch**.
5. **Installation Options:** Point to your Windows 10 Enterprise ISO.

### Step 3.2: Install Windows 10
1. Boot from the ISO and install Windows 10.
2. When prompted for a network/account setup, select **Domain join instead** (bottom left corner) to create a local offline account for now (e.g., username `localadmin`).

### Step 3.3: Point DNS to the Server
Before joining the domain, `CLIENT01` must know where to find `helpdesk.lab`.
1. Open `ncpa.cpl` -> Right-click adapter -> **Properties** -> **IPv4** -> **Properties**.
2. Select **Use the following DNS server addresses**.
3. **Preferred DNS:** Enter the static IP address of `DC01`.
   > *Why? If you skip this, Windows will ask your physical home router where "helpdesk.lab" is, and your home router has no idea.*
4. **Verify DNS & Connectivity:** Open Command Prompt and type `ping helpdesk.lab`.
   > *Why? `ping` sends a tiny packet of data and waits for a reply. If it replies with `DC01`'s IP address, it proves two things instantly: Your custom DNS setting works, and `CLIENT01` can successfully communicate with the server.*

### Step 3.4: Join the Domain
1. Press `Win + R` -> type `sysdm.cpl` -> hit Enter.
2. Click **Change...**
3. Select **Domain** and type `helpdesk.lab`.
4. Enter your credentials: `helpdesk\Administrator` and your server password.
5. You will see "Welcome to the helpdesk.lab domain." Restart the VM.

### Step 3.5: Log In
1. On the login screen, click **Other user**.
2. You can now log in using any user account created in your Active Directory (e.g., `helpdesk\jcarter`).

---

## Part 4: Automation (The Real-World Flex)
You don't need to manually recreate your OUs and users! 
1. Copy your `onboard-user.ps1` script from your GitHub repo to `DC01`.
2. Run it in PowerShell.
3. Instantly, all your users, security groups, and OUs will be rebuilt. 

You are now ready to install **Entra Connect** and sync them to the cloud!
