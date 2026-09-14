Preferred Servers are file shares that you establish, and Ivanti Neurons uses. No Ivanti code necessarily runs on a Preferred Server. A machine’s hosting of a preferred server share is independent of and unaffected by whether it is running any Ivanti software, such as a Neurons agent.

**Prerequisites**

A Preferred Server share must support the protocol scheme specified in its URL (see below). This is file (SMB), https or http.

**To add a preferred server**

1. In the Ivanti Neurons admin page, navigate to ==Admin > Preferred Server Settings > Servers.==
2. Click Add Server.  
	The Add Server details page displays.
3. Enter the name of the preferred server in the Preferred server name.
4. Specify the URL or path to an existing server.
	For example:
	- **Secure HTTP (Hypertext Transfer Protocol Secure)** (Recommended format)  
		**Example**: https://www.myserver.com/myshare  
		**Port**: Defaults to 443  
		**Encryption**: Yes - Encrypted via TLS/SSL  
		**Data privacy**: Secure; protects data in transit (encrypts against snooping or tampering).
		- **Plain HTTP (Hypertext Transfer Protocol)**  
		**Example**: http://www.myserver.com/share  
		**Port**: Defaults to 80  
		**Encryption**: None - everything is sent in plain text  
		**Data privacy**: Not secure - vulnerable to eavesdropping or man-in-the-middle attacks.
		- **File URI (Uniform Resource Identifier)**  
		**Example**: file://myserver/myfile  
		**Port**: N/A  
		**Encryption**: Minimal or no encryption; relies on OS-level permissions, e.g. NTFS or SMB.  
		**Data privacy**: Only if underlying protocol (for example, SMB 3.0+) supports it.
5. Select one of the existing Read credentials from the drop-down list. If not, create a new credential as follows:
	1. Click Create New Credentials.  
		The New Credential panel displays.
		2. Enter the Name of the credential.
		3. Enter the Description.
		4. Enter the Username.
		5. Enter the Password.
		6. Click Submit.
6. (Optional) Leave the Enable support for pre-configured Ivanti Security Controls distribution servers unselected.  
	This checkbox is for distribution servers being migrated to Ivanti Neurons for Patch also known as flat shares because all files are in the root of the share, without sub directories. If you are migrating an existing ISeC distribution server and want to continue using all the patch files already stored there, check this box.
	Do not enable this options, if any preferred server is used in a policy that has Distribution capability enabled.
7. (Optional) Specify **SMB Path** to ensure data consistency and availability across multiple systems.  
	The SMB path is used by the sync engines. For more information see [Configuring a sync engine](https://help.ivanti.com/ht/help/en_US/CLOUD/vNow/preferred-server-sync-engine.htm).
8. After entering all the information, select **Add Server**.  
	The new server appears in the Servers > Server list.