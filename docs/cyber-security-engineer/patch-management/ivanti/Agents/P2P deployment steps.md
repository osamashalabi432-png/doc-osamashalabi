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

**12.** Go to **Agents > Agent Policy > Create Policy** again.
**13.** Name it **P2P-Server**.
**14.** Turn on the **Patch Management** capability.
**15.** Go to **Platform Behaviors > Agent settings** tile.
**16.** Open **Peer download controls**. Choose **Client and Server**.
**17.** Set the **same** bandwidth and reboot settings as the Client policy.
**18.** Click **OK**, then **save** the policy.

### Part 3 – Link your patch configuration

**19.** Go to **Patch Management** and open your patch configuration.
**20.** Open the **Associations** tab.
**21.** Link it to **P2P-Client** and **P2P-Server**. Both of them.
**22.** Save.

### Part 4 – Assign the policies to devices

**23.** Go to **Agents > Agent Deployment**.
**24.** Open the **Automated policy assignment** tab.
**25.** Set the **Default policy** to **P2P-Client**. Now every device is a client.
**26.** Go to **Device exceptions**.
**27.** Add each server device from your list (step 3). Give each one **P2P-Server**.
**28.** Save.