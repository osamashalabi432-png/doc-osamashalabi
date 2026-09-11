# Delinea PAM — Discovery

## File inventory

File Inventory is a list of all apps and files your computers are using. The Privilege Manager agents scan your computers and send a list of the EXE/MSI/app files they find.

This is what feeds policy decisions — you can't write a sensible allow/deny/elevate policy for software you don't know is out there, so the inventory scan is effectively discovery for endpoint applications. It also pairs with the [VirusTotal integration](integrations.md): inventoried files can be checked for reputation before deciding how to treat them.
