For SABIL, the configuration would work like this:

1. Go to **Agents → Agent Policies → Create Policy**. Ivanti's Agent Policies documentation confirms that you create the policy there, select capabilities, then go to the **Agent settings** tab.      [Ivanti — Agent Policies](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policies.htm?utm_source=chatgpt.com)

2. Create your normal branch policy, for example `POL-BRANCH-ENDPOINT`. Enable the **Patch Management** capability. Then go to **Agent settings → Download Controls → Peer download controls** and choose **Client Only**. Ivanti defines Client Only as downloading content from peers but not sharing content with other peers.

3. Create another policy for the machines you want to act as your P2P sources, for example `POL-BRANCH-P2P`. Again enable **Patch Management**, then go to **Agent settings → Download Controls → Peer download controls** and choose **Client and Server**. That means the machine can both obtain content from peers and share cached content with other endpoints.

4. Ensure local firewalls allow **TCP 33121, TCP 33122, UDP 33121 and UDP 33122** between the peer machines and other devices. Ivanti explicitly requires these ports for peer downloads.      [Ivanti — Required URLs, IP addresses and ports](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/platform-allowlist.htm?utm_source=chatgpt.com)

5. Deploy the normal Ivanti Neurons Agent to both types of machines. There is **no separate P2P agent installer**. During manual or push installation, you select which Agent Policy the endpoint receives. Ivanti documents that under Agent Deployment.      [Ivanti — Agent Deployment](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-deployment.htm?utm_source=chatgpt.com)