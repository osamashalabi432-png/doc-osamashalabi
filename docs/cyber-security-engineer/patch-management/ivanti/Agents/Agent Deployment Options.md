The Ivanti Neurons Agent is required to use Ivanti Neurons platform. Use the following tabs to assign policy and push an agent on to an endpoint:

- ==Automated policy assignment:== Select to automatically manage and assign policies to devices in your environment according to configurable rules.
- ==Neurons push installation==: Select to download the agent and deploy to target devices.
- ==**Manual install**:== Select to download the agent and manually install via the command line on each target device.

## Automated Policy Assignment

Automated policy assignment (APA) automatically manages and assigns policies to devices in your environment based on configurable rules. With APA, administrators can streamline and centralize policy management, ensuring devices are always assigned the correct policy based on organizational criteria, device groups, exceptions, and other attributes.

You can use the APA feature to leverage the following key functions for assigning polices to endpoints:

- **Rule-Based Assignment**: Policies are assigned to devices according to customizable rules.
- **Group and Exception Handling**: Devices can be grouped, and exceptions can be configured.
- **Manual and Automated Modes**: Transition from manual policy assignments to rule-based automation at your pace.
- **24-hour Cool-down**: Limits policy re-assignments to avoid instability and resource usage.

To deploy APA, you have to configure the policy assignment initially. For more information about configuring APA, refer to [Configure Automated Policy Assignment](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/configure-automated-policy-assignment.htm).

## Manually Install Agent

To install an Ivanti Neurons agent manually, do the following:

1. Navigate to Agents > Agent Deployment, on the Manual installation tile, click Get started.  
	The Manually install an Agent page appears.
2. Choose your Agent Policy: From the drop-down, select the Agent Policy that you want to assign to the agent, or start typing the name of the policy to filter the drop-down list.  
	Or, select the **Use automated policy assignment** option from the drop-down to use the APA based deployment.  
	Agent Policies are created in Agents > Agent Policies.
3. Choose your Enrollment Key: From the drop-down, select the Enrollment Key that you want to use to register the agent. Alternatively to use a new key, select Create New Key. The [Create Enrollment Key](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/enrollment-keys.htm#create-enrollment-key) panel appears.  
	Enrollment keys are created in Agents > Enrollment Keys.  
	If you are using the automated policy assignment feature for agent deployment, an inbuilt auto generated key will be used to register the agent with the tenant. The enrollment key is set to **Auto-Generated** by default. You can also use your own customer enrollment key if required.
4. Choose the host endpoint system: Select the operating system for the device on which you are installing the agent:

After the agent has been installed on the endpoint, the device details are listed in the main menu > Devices.

If you use distribution software (such as Ivanti Endpoint Manager) that sends the arguments separately, remember to remove the agent file name from the copied text before you paste it into the distribution software. For details on how to use Ivanti Endpoint Manager to distribute the Ivanti Neurons Agent, see [How to deploy an Ivanti Neurons Agent using Endpoint Manager](https://forums.ivanti.com/s/article/How-to-deploy-an-Ivanti-Cloud-agent-using-Ivanti-Endpoint-Manager).

## Deploy Agent with Neurons Push Install

Ensure that the device name, IP address, and operating system information are available for the device so they can be selected or viewed from the list during a push install.

To deploy and install an Ivanti Neurons agent to an endpoint with Ivanti Neurons, do the following:

1. Navigate to Agents > Agent Deployment, on the Neurons push installation tile, click Get started.  
	The Deploy with Ivanti Neurons page appears.

### Deployment Settings

3. Agent Policy: From the drop-down, select the Agent Policy that you want to assign to the agent, or start typing the name of the policy to filter the drop-down list. Or, select the **Use automated policy assignment** option from the drop-down to use the APA based deployment.  
	Agent Policies are created in Agents > Agent Policies.
4. Enrollment Key (activations left): From the drop-down, select the Enrollment Key that you want to use to register the agent, or start typing the name of the enrollment key to filter the drop-down list. Ensure you have enough activations left for the number of devices you want to deploy to.  
	Alternatively, to use a new key, select Create New Key. The [Create Enrollment Key](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/enrollment-keys.htm#create-enrollment-key) panel appears.  
	Enrollment keys are created in Agents > Enrollment Keys.  
	If you are using the automated policy assignment feature for agent deployment, an inbuilt auto generated key will be used to register the agent with the tenant. The enrollment key is set to **Auto-Generated** by default. You can also use your own customer enrollment key if required.
5. Deploy From: Select the endpoint to use for the deployment. If there is at least one, but less than 500 agent endpoints with the Deployment capability enabled, the Automatic option is the default selection. You can select a specific endpoint from the drop-down list, or start typing the name of the endpoint to filter the drop-down list. Only endpoints that have the agent installed, with the Deployment capability enabled, are available for selection.

### Source Devices

The Source Devices section lists all devices that have a name, IP address, and operating system in the main Neurons Devices view.

The Non-Persistent VDI endpoints are currently unavailable for selection. To apply changes, these endpoints need to be unsealed, updated, and then resealed. Instead, deploy the agent policy directly to the NP-VDI image. For more information about VDI, see [Non-Persistent Virtual Device Infrastructure (NP-VDI)](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/non-persistent-vdi.htm) and [Persistent Virtual Device Interface (VDI) and Gold Image Cloned Devices](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/persistent-vdi.htm).

9. The default list is All Devices. You can filter the list by the following, or use the search to locate a specific device:
	- All Devices (default), default Device Groups: EPM Managed, Cloud Managed, Portable, public Device Groups, or your private Device Groups (created within Devices).
		- With or Without Agents.
10. Select the check box for each device you want to deploy the agent to.
11. Click Add to add the selected source devices to the Target Devices section.

### Target Devices

The Target Devices section lists all the devices that you have added from the Source Devices section. These are the devices that you have selected to deploy the agent to.  
The Device name, IP address and Operating system from the Devices record is displayed.

14. Click Deploy to start the deployment to all the target devices.  
	The deployment status can be seen in the [Status](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-management.htm#status) column on the Agent Management page.

If you need to remove a device from the target list, select the check box next to the device and click Remove. Alternatively, click Remove All to remove all devices from the target list.

### Credentials

Device credentials are required for the agent to perform actions on a device that require elevated privileges, or to scan for other devices on the network. You can not proceed with deployment until credentials have been added for the relevant OS being deployed to. The Credentials tab is highlighted in red until this is resolved.

18. Set the credentials for the relevant OS: Windows, macOS, Linux. From the drop-down, select a credential. To create new credentials, select Create New Credential. The New credential panel appears.