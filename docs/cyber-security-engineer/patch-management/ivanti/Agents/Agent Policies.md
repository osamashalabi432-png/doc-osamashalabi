The Agent Policies page displays a list of all system generated and custom policies.

- Policy: The agent policy name.
- Version: The number of times the policy has been updated. The updates might be from directly editing the agent policy or changing capability or configuration that is in the policy. For example, if you update Discovery settings, and the policy includes the Discovery capability, a new version is saved.
- Agent Endpoints: The number of agent endpoints that the policy has been assigned to.
- Modified Date: The date the policy was last amended.

## Actions

The following actions are available for each policy:

- ==View==: Select to display the Policy details page. You can view the Agent settings, Reboot experience, Capabilities, Enrollment Keys, and Agent Endpoints.
- ==Edit==: Select to display the Policy details page. You can edit the policy name and description, and the Capabilities selection.
- ==Delete==: Select to delete the policy. A confirmation dialog is displayed. Click Delete to confirm the action.  
	You cannot delete the predefined Infrastructure Agents policy.

[How to create an Agent Policy](#)

1. Navigate to ==Agents > Agent Policies==.  
	The Agent Policies page appears.
2. Select Create Policy.  
	The Create Agent Policy page appears.
3. Enter a Name for the policy.
4. Optionally enter a Description.
5. On the Capabilities tab, in the Available Capabilities section, select all of the [Agent Policy Capabilities](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policy-capabilities.htm) you want to enable for the policy. Any combination of capabilities can be added to a policy, for example Connectors, Deployment, Active Discovery, and Passive Discovery.
6. Click on the Agent settings tab to configure the [Agent Policy settings](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policy-settings.htm#Agent-settings).
7. Click on the Reboot experience tab to configure the [Agent Policy Reboot experience](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/agent-policy-reboot.htm#Reboot-experience).
8. Click on the Maintenance Windows tab to configure the [Maintenance Windows](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/Agent-policy-maintenance.htm).
9. Click Create Policy.
10. The agent policy is created and displayed the list on the Agent Policies page.

[How to edit an existing Agent Policy](#)

1. ==Navigate to Agents > Agent Policies==.  
	The Agent Policies page appears.
2. Select the Actions menu button for the policy you want to edit.  
	The drop-down menu appears.
3. Select Edit.  
	The Edit Agent Policy page appears.
4. You can edit the Name, Description, Capabilities, Agent settings, and Reboot experience.
5. Once you have made all the required edits, click Save.

[How to view an existing Agent Policy](#)

1. Navigate to Agents > Agent Policies.  
	The Agent Policies page appears.
2. Select the Actions menu button for the policy you want to view.  
	The drop-down menu appears.
3. Select View.  
	The Policy details page appears.  
	Alternatively, you can click on the Policy name in the list.
4. You can view all of the policy settings, enrollment keys and agent endpoints.
5. To make changes click the Edit.

[How to delete an Agent Policy](#)

1. Navigate to Agents > Agent Policies.  
	The Agent Policies page appears.
2. Select the Actions menu button for the policy you want to delete.  
	The drop-down menu appears.
3. Select Delete.  
	The Delete Policy confirmation dialog appears.
4. Click Delete to confirm the action.  
	Any associated enrollment keys will be revoked, however the number of activations will remain.
5. The policy is deleted, and is removed from the list on the Agent Policies page.