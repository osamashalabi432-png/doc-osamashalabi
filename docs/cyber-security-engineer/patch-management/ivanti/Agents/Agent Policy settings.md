To access the Agent settings panel, navigate to Agents > Agent Policies > Create Policy > Agent settings tab.

## Download Controls
[![Open](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/Skins/Default/Stylesheets/Images/transparent.gif)Peer download controls](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policy-settings.htm?utm_source=chatgpt.com#)

This allows devices on a network to share agent, engine and configuration installations between one another. One device can connect directly with another without going through an intermediary server. A peer-to-peer network performs more efficiently than a client-server network with the more devices you have, due to the file transfer load being distributed between them. It is also more reliable than a client-server network because it will remain functional if there is a server connection issue.

When using peer download, ensure your firewall allows ==UDP and TCP traffic on ports 33121 and 33122.==  
Peer-to-peer supports digitally signed and sideloaded patches. Patches automatically downloaded from the vendor that are not digitally signed, are not supported by peer-to-peer, for example, 7-Zip and Core FTP.

[![Open](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/Skins/Default/Stylesheets/Images/transparent.gif)Preferred server download controls](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policy-settings.htm?utm_source=chatgpt.com#)

If you are using only one preferred server, then you can point it to that preferred server, or you can assign it to a range of IP addresses for a more targeted download.

By adding multiple preferred servers, Ivanti Neurons scans for the downloaded content in all the preferred servers. If it fails to find the content, Ivanti Neurons automatically downloads the content in the order of peer, preferred servers, and other cloud servers.

Select the Download from preferred server toggle > select a Preferred Server from the drop-down or Detect by IP address.

Select from the following options:

- Disabled: Content will not be shared with or downloaded from peers.
- Client Only: Content will not be shared with peers. Content will be downloaded from peers.
- Server Only: Content will be shared with peers. Content will not be downloaded from peers.
- Client and Server: Content will be shared with and downloaded from peers.

## Bandwidth Utilization

Select the bandwidth utilization percentages. These limits will restrict how much of the network bandwidth can be used for Ivanti Neurons agent downloads. This can be used to prevent the agent consuming all of the bandwidth when used over limited or metered bandwidth connections, allowing other resources to utilize the network at the same time.

- LAN Utilization (%): Set the maximum allowed percentage between 10 -100. This throttles the network bandwidth allocated for downloading Ivanti Neurons agent and capabilities to the set percentage for the local area network (LAN). For multicast peers this is the local subnet only.  
	LAN ranges are determined as:
	- 10.0.0.0 - 10.255.255.255 (10/8 prefix)
		- 172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
		- 192.168.0.0 - 192.168.255.255 (192.168/16 prefix)
- WAN Utilization (%): Set the maximum allowed percentage between 10 -100. This throttles the network bandwidth allocated for downloading Ivanti Neurons agent and capabilities to the set percentage for the wide area network.

## Agent Automatic Update

The Ivanti Neurons Agent automatically updates itself with bug fixes and enhancements. Sometimes the updates require a reboot. Select how you'd like to handle these reboots:

- Request reboots when needed: Select to request a reboot when they are required.

This setting is required if using App Control.

- Do not request reboots: Select to not request a reboot. If reboots are not requested, some of the agent components may not be fully functional. A manual reboot will be required.