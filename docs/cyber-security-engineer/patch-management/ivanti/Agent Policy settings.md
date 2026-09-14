To access the Agent settings panel, navigate to Agents > Agent Policies > Create Policy > Agent settings tab.

## Download Controls

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