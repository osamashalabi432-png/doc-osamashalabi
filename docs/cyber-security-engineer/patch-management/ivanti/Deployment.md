### P2P
it's a toggle inside the **Agent Policy**, not inside the patch configuration. 

**Where it is**
Agents > Agent Policies > Create/Edit Policy > **Agent settings** tab > Download Controls > Peer download controls.

**The four modes** Disabled, Client Only (downloads from peers, doesn't share), Server Only (shares, doesn't download), Client and Server (both).

**What to actually pick**
- Branch/office desktops on the same subnet → Client and Server
- One or two stable machines per site you want as the source → Server Only
- Laptops, VPN/roaming users, DMZ or critical servers → Disabled

Peers only talk within the same subnet, so your policy design should follow your site/VLAN layout. One global policy defeats the purpose.

---
### Preferred Server per branch

A **Preferred Server** is a content share you host, an HTTPS or UNC (universal naming convention) path holding the patch files. Endpoints download from it instead of the vendor's internet servers.

A **Sync Engine** is what fills it. It's a capability you enable on an agent, like any other Neurons engine. It downloads the vendor files from the internet and uploads them to the preferred servers, and it deletes expired files too. It runs four times a day automatically, and you can trigger it manually from the command line.

So the flow becomes:

**Internet → Sync Engine → Preferred Server at the branch → all endpoints at that branch**

One download from the internet per branch instead of one per machine. That's your answer.

- prerequisite for it is having a SMB, HTTP/S protocols enabled to make it work

**To add a preferred server**

1. In the Ivanti Neurons admin page, navigate to **Admin > Preferred Server Settings > Servers.**
2. Click Add Server.  The Add Server details page displays.
3. Enter the name of the preferred server in the Preferred server name.
4. Specify the URL or path to an existing server.