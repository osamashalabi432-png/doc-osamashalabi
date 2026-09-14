To access the Agent Policy Capabilities navigate to Agents > Agent Policies > Agent Policy > Capabilities tab.

The available capabilities are license dependent, please refer to your Ivanti Neurons package for included product capabilities. To learn more about the product capabilities refer to the relevant Help topic.

Any combination of capabilities can be enabled in a policy. For example, Connectors, Deployment, Active Discovery, and Passive Discovery.

Deploying agent infrastructure capabilities to Non-persistent VDI endpoints is not recommended. Since these endpoints are temporary, critical Ivanti Neurons functions may become unavailable. For more information about NP-VDI, see [Non-Persistent Virtual Device Infrastructure (NP-VDI)](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/non-persistent-vdi.htm).

You can also deploy persistent VDI endpoints and gold image cloned devices to Ivanti Neurons. For more information about VDI, see [Persistent Virtual Device Interface (VDI) and Gold Image Cloned Devices](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/persistent-vdi.htm).

There is a tile for each available capability, select the check box for each capability you want to enable for the agent policy:

- Active Discovery: Enables discovery of devices by scanning IP ranges. Uses ICMP, NetBIOS, reverse DNS queries, OS detection, Remote inventory, and SNMP OIDs. Any devices outside of the IP range will be discovered by global discovery using remote inventory and SNMP.
- Agent UI: Neurons Agent end-user-interface component which allows local administrative tasks and limited reporting to the end user.
- App Control: Enables privilege and application control, providing endpoint security. Create configurations with application rules that can allow, deny, elevate and check for trusted vendors. Learn more about [App Control](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/app-control.htm).
	- Configuration: Select the App Control configuration to assign to the policy. The configuration must be in a Published state.
- Distribution: Downloads and installs applications as assigned in the App catalog.
- Application Control (Hybrid): Enables privilege and application control, providing endpoint security. Learn more in the [Application Control Help](https://help.ivanti.com/ap/help/en_US/am/2024/Content/Application_Manager/About_Application_Manager.htm).  
	Deploy Application Control configurations via Ivanti Neurons. Manage configurations via the on-premise Application Control console.
	- Configuration: Select the Application Control configuration to assign to the policy. The configuration controls which Application Control settings are deployed to the endpoint. Learn more about [Ivanti Neurons and Application Control integration](https://help.ivanti.com/ap/help/en_US/am/2024/Content/Application_Manager/Integrating-with-Neurons.htm "link to Application Control integrating with Ivanti Neurons").
	For this feature you may need to add URLs to the allow list of your firewall. For more information, see the [Accessing and interacting with the Neurons Platform portal](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/platform-allowlist.htm#UWM) section of the 'Required URLs, IP addresses and ports' topic.
- Automation: Enables Neurons Platform to communicate with a wide array of systems outside of Neurons Platform. It can be used to retrieve information or to perform tasks. The user experience revolves around three concepts: What, Who, When.  
	If Automation is disabled, you will not be able to run actions on those devices targeted by the policy from Ivanti Neurons Platform > Devices/Edge Intelligence/Neurons, including the creating and viewing of support tickets in ISM and ServiceNow.
- Connector Server: Enables the import of data from configured connectors. Connectors are configured on the [Connectors](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/connectors.htm) page.
	- Configuration: Select the configuration to assign to the policy.
- Deployment: Enables deployment of the Ivanti Neurons Agent and Policy to devices on the network. Secure device credentials are stored locally for use in agent management.
- Edge Intelligence: Provides real-time insights, as well as remediation and alerting capabilities for your environment. Data is retrieved from devices in real-time, at the moment you request it.  
	If Edge Intelligence is disabled, troubleshooting data will not be present in Devices. Certain functionality in People will also be impacted; latest location, Active Directory Status. Edge Intelligence and Neurons will not return data against those devices targeted by the policy.
- Environment Manager (Hybrid): Enables a consistent and portable user environment, delivering on-demand personalization and context-aware policy controls. Learn more in the [Environment Manager Help](https://help.ivanti.com/ap/help/en_US/em/2024/Content/Environment_Manager/LandingPage.htm).  
	Deploy Environment Manager configurations via Ivanti Neurons. Manage configurations via the on-premise Environment Manager console.
	- Configuration: Select the Environment Manager configuration to assign to the policy. The configuration controls which Environment Manager settings are deployed to the endpoint. Learn more about [Ivanti Neurons and Environment Manager integration](https://help.ivanti.com/ap/help/en_US/em/2024/Content/Environment_Manager/Integrating-with-Neurons.htm "link to Environment Manager intergating with Ivanti Neurons").
	For this feature you may need to add URLs to the allow list of your firewall. For more information, see the [Accessing and interacting with the Neurons Platform portal](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/platform-allowlist.htm#UWM) section of the 'Required URLs, IP addresses and ports' topic.
- Inventory Scanner: Enables the deep inventory scanner for the device. Enabled by default when creating a new policy.
- Passive Discovery: Enables discovery of connected devices on the subnet by listening to network traffic. Uses ARP, NetBIOS, reverse DNS queries, and OS detection. Includes CSEP if required, learn more about the [Client Self-Election Process](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/discovery-passive-settings.htm#Self-ele).
- Patch Management: Provides zero trust security capabilities and brings a continuous vulnerability management experience to help organizations manage and prioritize vulnerabilities, from detection through to remediation.
	- Configuration: Select the patch configuration to assign to the policy.
- Performance Manager (Hybrid): Allows system resources to be monitored and controlled, helping to deliver an optimal user experience. Learn more in the [Performance Manager Help](https://help.ivanti.com/ap/help/en_US/pm/2024/help/Performance_Manager/About_Performance_Manager.htm).  
	Deploy Performance Manager configurations via Ivanti Neurons. Manage configurations via the on-premise Performance Manager console.
	- Configuration: Select the Performance Manager configuration to assign to the policy. The configuration controls which Performance Manager settings are deployed to the endpoint. Learn more about [Ivanti Neurons and Performance Manager integration](https://help.ivanti.com/ap/help/en_US/pm/2024/help/Performance_Manager/Integrating-with-Neurons.htm "link to Performance Manager integrating with Ivanti Neurons").
	For this feature you may need to add URLs to the allow list of your firewall. For more information, see the [Accessing and interacting with the Neurons Platform portal](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/platform-allowlist.htm#UWM) section of the 'Required URLs, IP addresses and ports' topic.
- Power Management: Enables power management settings. An administrator can manage and optimize the power consumption of the devices in their environment. Administrators can now configure the power management settings and assign them to a policy.  
	Power Management is available on Windows devices only. Learn more about [Power Management Settings](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/power-management-settings.htm)
- Remote Control: Allows IT analysts to securely remote control endpoints so they can troubleshoot problems.