This page provides a brief overview of the steps involved in setting up Ivanti Neurons for Patch Management. There are two main options:

- **Cloud workflow**: the primary workflow for performing patch management functionality. All configuration and assessment activities are performed within the cloud, while the actual scans and deployments are performed by agents that are installed on the managed devices.
- **Hybrid workflow**: for customers using a [connector to an on-premise patch management product](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/connectors.htm) such as Ivanti Endpoint Manager, Ivanti Patch for Configuration Manager, Ivanti Security Controls, Ivanti Endpoint Security or Ivanti Desktop and Server Management.

For details of which operating systems and Linux distributions can be patched, see [Operating System Compatibility Matrix](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/compatibility.htm).

Patching of additional Linux distributions such as Rocky, Alma, and Debian, and extended support for distributions such as CentOS 6, CentOS 7, Oracle 6, Oracle 7, Ubuntu 16, and Ubuntu 18 are available through the partner program. For details, see **KernelCare Enterprise** on the [Ivanti Marketplace](https://marketplace.ivanti.com/xchange/64b5b48e2ee9d6e08c3e6724/solution/66633621c0e1edac7610ba53).

## Cloud Workflow

This is the primary workflow for performing patch management functionality. All configuration and assessment activities are performed within the cloud, while the actual scans and deployments are performed by agents that are installed on the managed devices.

![1) Create a custom policy group 2) Create a patch configuration and associate it with you policy group 3) Wait for the changes to propagate to your agents 4) Agents scan for and deploy patches 5) Results are reported to Ivanti Neurons.](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/Images/PrimaryWorkflow-L10N.png)

<table><colgroup><col> <col></colgroup><thead><tr><th colspan="2"><p>Items in Figure</p></th></tr></thead><tbody><tr><td><p>A</p></td><td><p>Agent devices</p></td></tr><tr><td colspan="2"><p>Cloud Workflow</p></td></tr><tr><td><p>1</p></td><td><p>Create a custom policy.</p></td></tr><tr><td><p>2</p></td><td><p>Create a patch configuration and associate it with your policy.</p></td></tr><tr><td><p>3</p></td><td><p>Wait for the changes to propagate to your agent devices.</p></td></tr><tr><td><p>4</p></td><td><p>Agents scan for and deploy patches on the managed devices.</p></td></tr><tr><td><p>5</p></td><td><p>Scan and deployment results are reported to Ivanti Neurons.</p></td></tr></tbody></table>

### Details of the Cloud Workflow

#### Conditional Steps

If your devices already contain an Ivanti Neurons agent, you can skip these conditional steps.

1. [Download an agent](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-deployment.htm) for the appropriate device type (Windows, Mac, or Linux).  
	There are two files included in the download:
	- The agent executable file
		- An options file that contains the tenant ID, enrollment key, and cloudhost information that will be needed during the installation process
2. [Install the agent](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-deployment.htm) on the desired target devices.
	1. On the target device, double-click the executable file to begin the installation process.
		2. Follow the instructions in the installation wizard.
3. Wait for the agent to automatically do the following:
	- Register and check in with Ivanti Neurons
		- Download the assigned [agent policy](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policies.htm)
		- Perform a scan of the target device for all missing patches and report the results to Ivanti Neurons
4. View information about the newly discovered target devices from [Devices view](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/devices.htm) within Ivanti Neurons.

#### Primary Steps

1. Create a custom [agent policy](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policies.htm).  
	The default policy that is initially installed with an agent is configured to perform only patch scans. In order to perform patch deployments, you need to create at least one custom policy. The custom policy must be enabled to perform patch management actions and it must be associated with a patch configuration that defines your deployment settings. You use the Add Devices option within the policy to assign the policy to the desired agent devices.  
	Be sure to enable the Patch Management capability when creating the agent policy. The policy defines the rules that allow the agent to operate autonomously on a device, with or without human interaction.  
	Within the custom agent policy you can select to use peer-to-peer download. Peer-to-peer supports digitally signed and sideloaded patches. Patches automatically downloaded from the vendor that are not digitally signed, are not supported by peer-to-peer, for example, 7-Zip and Core FTP. The server peer will share only OS applicable patches to the peer client, for example a Server 2019 will share only 2019 patches.
2. Configure your [patch settings](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patching-settings.htm).  
	Your patch settings will consist of the following:
	- Patch configuration: The primary purpose of a patch configuration is to define how patches will be deployed to the agent devices. A default patch configuration is provided that will deploy all missing critical security patches in your Windows environment on a weekly basis. You will likely want to create one or more custom patch configurations to define the unique patch deployment requirements of your organization.
		On the Associations tab, be sure to associate your patch configuration with a custom agent policy that is enabled to perform patch management actions.
		- (Optional) Patch group: You may choose to reference a patch group in a patch configuration. A patch group contains a list of specific patches that you want to deploy. This is a good way to make sure that only approved patches are deployed. See the Deployment behavior section in the [Patch Settings](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patching-settings.htm) topic for information on how to properly configure this scenario.
3. Wait for the changes to propagate to your devices.  
	Your devices will receive the updated policy and patch configuration information the next time the agents check in with Ivanti Neurons.
4. Deploy missing patches to the agent device.  
	The deployment can be accomplished four different ways:
	- Via an automatic, scheduled patch deployment that is defined by the [patch configuration](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patching-settings.htm).
		- From the [Endpoint Vulnerability](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/endpoint-vulnerability.htm) component within Ivanti Neurons for Patch Management. You can use this component to deploy all patches that were identified as missing during the most recent patch scan.
		Be sure you have the Patch Management [permissions](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/access-control.htm) needed to deploy patches from Endpoint Vulnerability.
		- From the Patches tab of the [Device Details](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/device-details.htm) page. You can select individual patches for deployment from this page.
		- By using the [Agent UI](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-client.htm) on the agent device to immediately initiate a patch deployment.
	To install the Agent UI you must enable the capability in the assigned agent policy.
	If a patch fails to install, it is re-tried (Windows: up to three times in an individual patch cycle and up to five times in total for the endpoint. Mac: up to three times. Linux: no retry). You can also configure the deployment to run on reboot as part of the configuration's schedule if the device is offline.
	After a patch deployment, the agent device will be automatically rescanned and the results sent to Ivanti Neurons. This will enable you to verify the deployment status and assess the current health of the agent device.
5. Use the [Deployment History](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/deployment-history.htm) component to view the results of the deployment and quickly identify any issues.
6. Use the [Endpoint Vulnerability](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/endpoint-vulnerability.htm) component to assess the patch health of the devices in your environment.
7. Use the [Patch Intelligence](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patch-intelligence.htm) component to gain a deeper level of understanding about the vulnerabilities detected on your devices based on risk-based prioritization, patch reliability and patch compliance.

## Hybrid Workflow

This workflow applies to current customers who are utilizing a [connector to an on-premise patch management product](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/connectors.htm) such as Ivanti Endpoint Manager, Ivanti Patch for Configuration Manager, Ivanti Security Controls, Ivanti Endpoint Security or Ivanti Desktop and Server Management. These customers will begin the migration to the Cloud workflow by simultaneously utilizing both workflows. For example, existing customers might choose to transition the management of their workstations to the Cloud workflow, while continuing to provide patch management capabilities to disconnected workstations, servers and other high-profile or sensitive devices using their on-premise workflow. This strategy enables you to transition to the Cloud workflow at your own pace.

![The on-premise console scans for and deploys patches to the managed machines. The results are reported to Ivanti Neurons.](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/Images/PrimaryAndHybridWorkflows-L10N.png)

<table><colgroup><col> <col></colgroup><thead><tr><th colspan="2"><p>Items in Figure</p></th></tr></thead><tbody><tr><td><p>A</p></td><td><p>Agent devices</p></td></tr><tr><td><p>B</p></td><td><p>On-premise patch management console (Endpoint Manager, Security Controls, etc.)</p></td></tr><tr><td><p>C</p></td><td><p>Console-managed devices</p></td></tr><tr><td colspan="2"><p>Cloud Workflow</p></td></tr><tr><td><p>1</p></td><td><p>Create a custom policy.</p></td></tr><tr><td><p>2</p></td><td><p>Create a patch configuration and associate it with your policy.</p></td></tr><tr><td><p>3</p></td><td><p>Wait for the changes to propagate to your agent devices.</p></td></tr><tr><td><p>4</p></td><td><p>Agents scan for and deploy patches on the managed devices.</p></td></tr><tr><td><p>5</p></td><td><p>Scan and deployment results are reported to Ivanti Neurons.</p></td></tr><tr><td colspan="2"><p>Hybrid Workflow</p></td></tr><tr><td><p>i</p></td><td><p>Connector</p></td></tr><tr><td><p>ii</p></td><td><p>The console scans for and deploys patches to the managed devices.</p></td></tr><tr><td><p>iii</p></td><td><p>Console results are reported to Ivanti Neurons.</p></td></tr></tbody></table>

The result will be a combination of data in the Ivanti Neurons Platform for both cloud and on-premise managed devices. Patch deployments for endpoints governed by the Cloud workflow will be performed by the Ivanti Neurons agent. Deployments to devices governed by an on-premise solution will continue to be performed by the on-premise console.

It is possible that you may have devices that are managed by both the Ivanti Neurons Cloud and an on-premise solution. Both solutions will work effectively side by side. Actions performed from Ivanti Neurons Cloud will take precedence because they provide direct interaction with the devices.

While it is possible to have devices managed by both solutions, it probably is not desirable, as receiving multiple reports from different products can become confusing.

The deployments can be initiated by an Ivanti Neurons agent, by an Ivanti Endpoint Manager or Ivanti Security Controls on-premise console, or from within the cloud using either the [Device Details](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/device-details.htm) or [Endpoint Vulnerability](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/endpoint-vulnerability.htm) components.

If you are using a custom scan profile in Ivanti Security Controls, you may find when deploying a missing patch that you are told it is already installed on the device. To avoid this, add a regular scan of all devices using one of the predefined Security Patch Scan or All Patches templates. For more information, see this [Ivanti Community Article](https://forums.ivanti.com/s/article/Patch-Status-imported-via-ISEC-Connector-into-Neurons-does-not-fully-reflect-the-current-installation-status-in-ISEC) (opens in a new window).

## Patch logs

For all patch deployments, the Neurons patch engine records activities and data in multiple log files. The default log file size is set to 20MB and two rollover log files created to handle larger data volumes.

Ivanti provides the following options to configure these settings:

- Modify the registry keys: HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Ivanti\\Ivanti Cloud Agent\\\<endpoint architecture>.
	- MaximumNumberOfLogFiles: REG-SZ
		- MaximumLogFileSize: REG-SZ

Changes to registry values take effect only after the patch engine is upgraded.

- Apply changes immediately: Manually update the configuration file values in STPatch.exe.config file located in the install directory (typically, C:\\Program Files\\Ivanti\\Ivanti Cloud Agent\\\<endpoint architecture>).
	- maximumFileSize
		- maximumNumberOfFiles

Use the appropriate name based on the endpoint architecture:

- x86 - AGENTPATCH
- x64 - AGENTPATCH64
- ARM64 - AGENTPATCHARM64

This feature is enabled by default in all patch configurations. However, older configurations may need to be republished to agent management to apply the updated behavior.

### Handling Sandbox log folders

During patch deployment, the system creates temporary sandbox folders to capture deployment activity for each patch deployment. These folders contain log files and execution artifacts, and the folders are timestamped. By default, the installation media and other temporary files are retained for 7 days, and other log files are retained for 60 days.

To preserve sandbox data for troubleshooting or future analysis, rename and archive the folders to a different location before they are automatically removed.