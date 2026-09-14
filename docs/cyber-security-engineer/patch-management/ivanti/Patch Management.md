Ivanti Neurons for Patch Management is a cloud patching solution. It combines the real-time insights of the Ivanti Neurons Platform with the asset information of Ivanti Neurons for Discovery and the actionable intelligence for risk-based prioritization to drive an adaptive security strategy. Comprehensive patch management capabilities are provided for your Windows, macOS, and Linux devices and includes the ability to patch products from both Microsoft, Apple, and third-party vendors.

To access Ivanti Neurons for Patch Management, navigate to Patch Management in the Ivanti Neurons Platform.

Ivanti Neurons for Patch Management comprises the following components, depending on your license:

- [Compliance Reporting](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/compliance-reporting.htm): Enables you to determine your current compliance status and see how you are trending over time.
- [Deployment History](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/deployment-history.htm): Provides a way to view the status of recent deployment operations. You can zero in on exceptions and quickly troubleshoot any issues.
- [Endpoint Vulnerability](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/endpoint-vulnerability.htm): Provides a central view of device patching for your environment with device health and risk-based metrics.
- [Patch Intelligence](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patch-intelligence.htm): Gathers and aggregates data to help manage, prioritize and streamline patching in your environment. It provides a clear picture of your threat landscape with prioritized, risk-based metrics.
- [Patch Settings](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patching-settings.htm): Enables you to configure patch configurations and patch groups for the cloud patch management workflow. A default configuration that remediates all critical security patches can be used to quickly get you started, or you can create a custom patch configuration to meet the unique compliance thresholds in your organization.
- [Ring Deployments](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/ring-deployments.htm): Enables you to manage ring deployments so that you can test the deployment of patches on a smaller number of test devices before continuing the deployment to all devices.
- [Patch for Intune](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patch-for-MEM.htm): Extends Microsoft Intune implementations to include third-party product management capabilities.
- [Reports](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/reports.htm): Enables you to create reports that contain data taken from Ivanti Neurons that you can use or distribute as PDF, CSV, or Excel files to people who do not have access to the system.

Be sure you have the [Access Control](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/access-control.htm) needed to use Patch Management.

## Requirements

There are a number of requirements to use Patch Management.

### Required URLs, IP addresses and ports

You must add a number of web URLs to your firewall, proxy and web filter exception lists. The URLs are used to download patch content from third-party vendors.

For the complete list of URLs that you need to add, see [Required URLs, IP addresses and ports](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/platform-allowlist.htm).

### Microsoft Windows and Microsoft Office

To successfully deploy patches with the Ivanti Neurons Agent to Windows devices, do not disable the Windows Update service, but set it to either Manual or Automatic. In addition, set the Windows Update setting on each target device (Control Panel > System and Security > Windows Update > Change settings) to Never check for updates. For more information, [see this article](https://forums.ivanti.com/s/article/Best-Practice-Windows-Automatic-Updates) on the Ivanti Community.

If you are patching Office 2019 or Office 365 that use Click-to-Run technology, see [How Ivanti patches Office Click-to-Run installations](https://forums.ivanti.com/s/article/How-Security-Controls-patches-Office-Click-to-Run-installations) on the Ivanti Community (opens in a new window) for information about how Patch for Neurons patches these installations.

Patch management is now available on Windows 11 IoT Enterprise LTSC.

### Microsoft’s Malicious Software Removal Tool (MSRT)

Microsoft’s Malicious Software Removal Tool (MSRT) is a commonly used security utility that helps detect and remove prevalent malware from Windows endpoints. Ivanti Neurons for Patch Management enables administrators to deploy MSRT directly from the console, enabling the delivery of the Security Tool as part of regular patch cycles.

To deploy an MSRT patch through a Routine Maintenance, Priority Updates, or Zero-Day Response task:

1. Create a Patch Group that contains the MSRT patch. For more information, see [Patch Groups](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patchintell-patchgroups.htm).
2. Add the Patch Group to the task's Deploy/Exclude by Patch Group configuration. For more information, see [Deployment By](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/patch-configuration-behavior.htm#Deployme) section.

When Microsoft releases a new version of MSRT, you must update the Patch Group to include the newly released patch in order for future deployments to remain current.

You can also deploy an MSRT patch manually from a device's Patch View if the patch is reported as missing.

### macOS

Ivanti Neurons for Patch Management is now supported on devices running macOS26 Taohe.

On Apple Silicon Macs and Intel Macs with the **Apple T2 chip**, Ivanti Neurons for Patch Management requires a dedicated **role account** to manage operating system patches on the device.

When Ivanti Neurons for Patch Management initiates the deployment of an OS patch for the first time, a system dialog appears prompting the local administrator to create a role account. On-screen instructions guide the administrator through the process. Administrative credentials are required to authorize the creation of the account.

Once the administrator completes the form, a new user account named \_ivantiNeuronsMacPatchAgent is created. A randomly generated password, unique to the device, is assigned to the account. The credentials are securely stored in the macOS Keychain.

When a macOS patch is required, Ivanti Neurons uses the \_ivantiNeuronsMacPatchAgent account to run the **systemupdate** utility. This utility applies the latest OS updates using the **InstallAssist.pkg** package retrieved from the Apple Content Delivery Network (CDN).

#### macOS devices managed with MDM

A local administrator with both Volume Owner permissions and a Secure Token can create additional users with the same privileges on the device. However, this is not the case for accounts provisioned via Entra ID or other Active Directory services. These accounts are not considered local to the machine. While they may have a Secure Token, they typically do not have Volume Owner permissions.

For MDM-managed devices, Volume Owner permissions are not required to perform administrative tasks such as user creation or OS patching. Instead, these devices leverage the **Bootstrap Token**, a mechanism designed to facilitate secure operations without relying on Volume Owner privileges.

For more information, see: [Use secure token, bootstrap token, and volume ownership in deployments](https://support.apple.com/guide/deployment/use-secure-and-bootstrap-tokens-dep24dbdcf9e/web).

As MDM-managed admin accounts cannot assign Volume Owner permissions, we recommend using MDM for deploying and managing macOS updates.

For MDM-managed devices, the \_ivantiNeuronsMacPatchAgent role account is not created. The Neurons patch engine cannot install OS patches on these endpoints. In such cases, the Patch Deployment History in Ivanti Neurons will display the OS patch entry with the status MDM configured: patch skipped.

While implementation details may vary across MDM providers, Ivanti Neurons for MDM supports this workflow. For more information see latest version of the [Ivanti Neurons for MDM documentation](https://www.ivanti.com/support/product-documentation).