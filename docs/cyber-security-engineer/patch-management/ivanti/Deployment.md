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

