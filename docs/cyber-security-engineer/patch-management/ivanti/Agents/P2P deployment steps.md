## Part 1 – Create the Client policy

- Go to **Agents > Agent Policy > Create Policy**
- Name it `P2P-Client`
- Turn on the **Patch Management** capability
- Go to **Platform Behaviors > Agent settings** tile
- Open **Peer download controls** → choose **Client Only**
- Set bandwidth:
    1. LAN Utilization: **50%**
    2. WAN Utilization: **20–30%**
    
- Agent Automatic Update → **Request reboots when needed**
- Click **OK**, then **save** the policy

## Part 2 – Create the Server policy

- Go to **Agents > Agent Policy > Create Policy**
- Name it `P2P-Server`
- Turn on the **Patch Management** capability
- Go to **Platform Behaviors > Agent settings** tile
- Open **Peer download controls** → choose **Client and Server**
- Use the **same** bandwidth and reboot settings as `P2P-Client`
- Click **OK**, then **save** the policy

> [!tip] Both policies should be identical. The **only** difference is the peer mode.

## Part 3 – Link the patch configuration

- Go to **Patch Management** → open your patch configuration
- Open the **Associations** tab
- Link it to **both** `P2P-Client` and `P2P-Server`
- Save

## Part 4 – Assign the policies to devices

- Go to **Agents > Agent Deployment**
- Open the **Automated policy assignment** tab
- Set **Default policy** → `P2P-Client` (every device becomes a client)
- Go to **Device exceptions**
- Add the one server device you picked in each subnet → give it `P2P-Server`
- Save

> [!note] Device exceptions override the default policy. That's how only the chosen device becomes the server.

## Part 5 – Check it worked

- Wait for devices to check in (can take a little time)
- Open the agent management list and check a few devices:
    1. Server devices → `P2P-Server`
    2. All others → `P2P-Client`
- Deploy a test patch to one subnet and watch whether the clients get it