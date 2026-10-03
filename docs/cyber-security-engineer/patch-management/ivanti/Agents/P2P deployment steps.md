### Open the Agent settings Tool

**Step 1 – Open the agent policy settings**
Go to Agents > Agent Policy > Create Policy > Platform Behaviors > Agent settings tile. [ivanti](https://help.ivanti.com/ht/help/en_us/CLOUD/vNow/agent-policy-settings.htm)
(On some versions, the path is Agents > Agent Policies > Create Policy > Agent settings tab.)


**Step 2 – Turn on Patch Management in the policy**
Make sure the Patch Management capability is enabled when you create the agent policy. [ivanti](https://help.ivanti.com/ht/help/en_US/cloud/vnow/patch-getting-started.htm)


**Step 3 – Pick a peer mode under "Peer Download Controls"**

The main options are:
- **Disabled** – no sharing at all
- **Client only** – the device downloads from peers, but does not share
- **Server only** – the device shares with peers, but does not download from them

These three modes are listed in Ivanti's docs. Your version may also show a combined option. [ivanti](https://help.ivanti.com/ht/help/it_IT/CLOUD/vNow/agent-policy-settings.htm)

**Step 4 – Set bandwidth limits (optional)**

You can limit how much of the network the agent uses. There's a separate percentage for the local network (LAN) and for the wide network (WAN). [ivanti](https://help.ivanti.com/ht/help/en_US/cloud/vnow/agent-policy-settings.htm)

**Step 5 – Link your patch configuration to the policy**

On the Associations tab, connect your patch configuration to the custom agent policy that has patch management turned on. [ivanti](https://help.ivanti.com/ht/help/en_US/cloud/vnow/patch-getting-started.htm)

Devices pick up the new settings the next time their agents check in    [Ivanti — Agent Deployment](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-deployment.htm?utm_source=chatgpt.com)