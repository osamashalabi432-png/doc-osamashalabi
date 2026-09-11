# Understanding General Concepts

Before hardening a Windows host, it helps to understand the core subsystems that both legitimate administration and malicious activity rely on — because these are also where a lot of the visibility (and abuse) happens.

## Services

Windows Services create and manage critical background functions — network connectivity, storage, memory, sound, user credentials, and data backup — running automatically without user interaction.

Access the Services console by typing `services.msc` in the Run window.

## Windows Registry

The Windows Registry is a unified database that stores configuration settings, essential keys, and shared preferences for Windows and third-party applications.

!!! warning
    Malicious programs commonly make undesired changes in the registry to abuse a program or service as part of routine system activity — registry persistence is a very common technique, so unexpected registry changes are worth investigating.

Access the Registry Editor by typing `regedit` in the Run dialog or taskbar search.

## Event Viewer

Event Viewer shows log details about all events occurring on the computer, including driver updates, hardware failures, changes in the operating system, invalid authentication attempts, and application crash logs.

Event Viewer receives notifications from different services and applications running on the computer and stores them in a centralized database. Event categories include:

- **Application** — records events of already-installed programs.
- **System** — records events of system components.
- **Security** — logs events related to security and authentication, etc.

Access Event Viewer by typing `eventvwr` in the Run window.

## Telemetry

Telemetry is a data collection system used by Microsoft to enhance the user experience by preemptively identifying security and functional issues in software. It's a service available in Windows and runs through `diagtrack.dll`.
