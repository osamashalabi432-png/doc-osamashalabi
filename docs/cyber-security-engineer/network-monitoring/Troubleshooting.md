### Core Server

- to access the logs on the core PRTG: `C:\ProgramData\Paessler\PRTG Network Monitor\Logs`
- this is a hidden folder so keep it in mind

- to see if the PRTG is running or stopped: `sc query PRTGCoreService`

- to see if the program is actually open or not: `tasklist | findstr /i prtg`

- check if the core has an IP or not: `ipconfig`

