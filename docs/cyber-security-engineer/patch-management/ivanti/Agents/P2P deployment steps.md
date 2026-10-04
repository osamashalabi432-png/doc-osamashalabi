### Part 1 – Create the Client policy

**1.** Go to **Agents > Agent Policy > Create Policy**.
**2.** Name it **P2P-Client**.
**3.** Turn on the **Patch Management** capability.
**4.** Go to **Platform Behaviors > Agent settings** tile.
**5.** Open **Peer download controls**. Choose **Client Only**.
**6.** Set bandwidth:

- LAN Utilization: **50%**
- WAN Utilization: **20–30%**

**7.** Agent Automatic Update: choose **Request reboots when needed**.
**8.** Click **OK**, then **save** the policy.

### Part 2 – Create the Server policy

**1.** Go to **Agents > Agent Policy > Create Policy** again.
**2.** Name it **P2P-Server**.
**3.** Turn on the **Patch Management** capability.
**4.** Go to **Platform Behaviors > Agent settings** tile.
**5.** Open **Peer download controls**. Choose **Client and Server**.
**6.** Set the **same** bandwidth and reboot settings as the Client policy.
**7.** Click **OK**, then **save** the policy.

### Part 3 – Link your patch configuration

**1.** Go to **Patch Management** and open your patch configuration.
**2.** Open the **Associations** tab.
**3.** Link it to **P2P-Client** and **P2P-Server**. Both of them.
**4.** Save.

### Part 4 – Assign the policies to devices

**1.** Go to **Agents > Agent Deployment**.
**2.** Open the **Automated policy assignment** tab.
**3.** Set the **Default policy** to **P2P-Client**. Now every device is a client.
**4.** Go to **Device exceptions**.
**5.** Add each server device from your list (step 3). Give each one **P2P-Server**.
**6.** Save.

### Part 7 – Check it worked

**1.** Wait for the devices to check in. This can take a little time.
**2.** Open the agent management list and check a few devices:
- Server devices should show **P2P-Server**
- All others should show **P2P-Client**

**3.** Deploy a test patch to one subnet. Watch whether the clients get it.