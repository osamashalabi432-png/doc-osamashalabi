### Open the Agent settings Tool

**Step 1 – Open the agent policy settings**

 - Go to Agents > Agent Policy > Create Policy > Platform Behaviors > Agent settings tile. [ivanti](https://help.ivanti.com/ht/help/en_us/CLOUD/vNow/agent-policy-settings.htm)
(On some versions, the path is Agents > Agent Policies > Create Policy > Agent settings tab.)

- Click the **Peer download controls** section to open it.


- Pick a peer mode under "Peer Download Controls"

Step 1 – Create the "Client" policy

Create a new agent policy. Name it something clear, like **P2P-Client**.

In the Agent settings tile, set peer mode to **Client Only**.

Turn on **Patch Management**, set your bandwidth limits, and save.

### Step 2 – Create the "Server" policy

Create a second policy. Name it **P2P-Server**.

Set peer mode to **Client and Server**.

Keep everything else the **same** as the Client policy. Same capabilities, same bandwidth, same reboot setting. The only difference should be the peer mode.

Save it.

**Step 4 – Set bandwidth limits (optional)**

You can limit how much of the network the agent uses. There's a separate percentage for the local network (LAN) and for the wide network (WAN). [ivanti](https://help.ivanti.com/ht/help/en_US/cloud/vnow/agent-policy-settings.htm)

**Step 5 – Link your patch configuration to the policy**

On the Associations tab, connect your patch configuration to the custom agent policy that has patch management turned on. [ivanti](https://help.ivanti.com/ht/help/en_US/cloud/vnow/patch-getting-started.htm)

Devices pick up the new settings the next time their agents check in    [Ivanti — Agent Deployment](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-deployment.htm?utm_source=chatgpt.com)