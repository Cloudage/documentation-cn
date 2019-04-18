# Unity Hub advanced deployment considerations

This topic provides information to help you plan and configure your Unity Hub deployment to work with your choice of deployment system (for example, Microsoft SCCM). It covers:

* [Firewall configuration](#firewall)
* [Silent install and uninstall on Windows](#silent)
* [Determining whether Unity Hub is installed on Windows](#installed)

<a name="firewall"></a>
## Firewall configuration

The Unity Hub uses the following ports:

* Outbound TCP Port 443 (HTTPS): The Hub uses this port to connect to Unity Services, providing functionality such as user login, Unity Editor updates, Unity Collaborate, and updates for the Hub itself. If you do not open this port, the Unity Hub is still usable, but online features are disabled.
* Outbound TCP/UDP Port 53 (DNS): The Hub uses this port to quickly determine whether an internet connection is available. If you do not open this port, the Hub assumes that no internet connection is available, even if Port 443 is open.
* Inbound TCP port 39000, accepting connections from localhost only: The Hub listens on this port to accept connections from Editor instances running on the local machine. This port must be open for connections from localhost.

<a name="silent"></a>
## Silent install and uninstall on Windows

You can install and uninstall the Hub from the command line using the silent-mode parameter. Silent-mode installs the Hub without prompting for user input. Local administrator access is required, because the installation modifies the Program Files folder.

To install the Unity Hub using silent-mode:

1. Download the [Unity Hub](https://store.unity.com/download?ref=personal).
2. Open a command prompt and navigate to the folder in which you downloaded the Hub.
3. At the command line, run the following command, in which the `/S` initiates a silent installation:

```
UnityHubSetup.exe /S
```

To uninstall the Unity Hub silently, pass the `/S` command-line switch to the uninstaller:

1. Open a command prompt.
2. At the command line, run the following command, in which the /S initiates a silent uninstall:

```
"C:\Program Files\Unity Hub\Uninstall Unity Hub.exe" /S
```

<a name="installed"></a>
## Determining whether Unity Hub is installed on Windows

To determine if the Unity Hub is installed, check for the presence of the following registry key:

```
HKEY_LOCAL_MACHINE\Software\Unity Technologies\Hub
```

If this key is present in the registry, the Unity Hub is installed.

------------------------------
<span class="page-edit">2018-06-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span>
