# Complete list of environment variables on Windows 10 - Pureinfotech
[Complete list of environment variables on Windows 10 - Pureinfotech](https://pureinfotech.com/list-environment-variables-windows-10/) 

 On [Windows 10](https://pureinfotech.com/tag/windows-10), environment variables are predefined names representing the path to certain locations within the operating system, such as a drive or a particular file or folder.

Environment variables can be helpful in many scenarios, but they’re particularly useful if you’re an IT person or you’re fixing someone else’s computer, as you can quickly navigate to certain locations without even knowing the username or full path to a system folder.

For example, instead of browsing a path like **“C:\\Users\\<UserName>\\AppData\\Roaming,”** you can open the **Run** command (Windows key + R), type this variable **“%APPDATA%,”** and press **Enter** to access the same path. Or you can use the **“%HOMEPATH%”** variable to access the current user default folders location — where the operating system stores the folders for Desktop, Documents, Downloads, OneDrive, etc.

In this [guide](https://pureinfotech.com/tag/how-to/), you’ll learn the list of the most common environment variables you can use on Windows 10.

Windows 10 default environment variables
----------------------------------------

| Variable | Windows 10 |
| --- | --- |
| %ALLUSERSPROFILE% | C:\\ProgramData |
| %APPDATA% | C:\\Users\\{username}\\AppData\\Roaming |
| %COMMONPROGRAMFILES% | C:\\Program Files\\Common Files |
| %COMMONPROGRAMFILES(x86)% | C:\\Program Files (x86)\\Common Files |
| %CommonProgramW6432% | C:\\Program Files\\Common Files |
| %COMSPEC% | C:\\Windows\\System32\\cmd.exe |
| %HOMEDRIVE% | C:\\ |
| %HOMEPATH% | C:\\Users\\{username} |
| %LOCALAPPDATA% | C:\\Users\\{username}\\AppData\\Local |
| %LOGONSERVER% | \\\\{domain\_logon\_server} |
| %PATH% | C:\\Windows\\system32;C:\\Windows;C:\\Windows\\System32\\Wbem |
| %PathExt% | .com;.exe;.bat;.cmd;.vbs;.vbe;.js;.jse;.wsf;.wsh;.msc |
| %PROGRAMDATA% | C:\\ProgramData |
| %PROGRAMFILES% | C:\\Program Files |
| %ProgramW6432% | C:\\Program Files |
| %PROGRAMFILES(X86)% | C:\\Program Files (x86) |
| %PROMPT% | $P$G |
| %SystemDrive% | C: |
| %SystemRoot% | C:\\Windows |
| %TEMP% | C:\\Users\\{username}\\AppData\\Local\\Temp |
| %TMP% | C:\\Users\\{username}\\AppData\\Local\\Temp |
| %USERDOMAIN% | Userdomain associated with current user. |
| %USERDOMAIN\_ROAMINGPROFILE% | Userdomain associated with roaming profile. |
| %USERNAME% | {username} |
| %USERPROFILE% | C:\\Users\\{username} |
| %WINDIR% | C:\\Windows |
| %PUBLIC% | C:\\Users\\Public |
| %PSModulePath% | %SystemRoot%\\system32\\WindowsPowerShell\\v1.0\\Modules\\ |
| %OneDrive% | C:\\Users\\{username}\\OneDrive |
| %DriverData% | C:\\Windows\\System32\\Drivers\\DriverData |
| %CD% | Outputs current directory path. (Command Prompt.) |
| %CMDCMDLINE% | Outputs command line used to launch current Command Prompt session. (Command Prompt.) |
| %CMDEXTVERSION% | Outputs the number of current command processor extensions. (Command Prompt.) |
| %COMPUTERNAME% | Outputs the system name. |
| %DATE% | Outputs current date. (Command Prompt.) |
| %TIME% | Outputs time. (Command Prompt.) |
| %ERRORLEVEL% | Outputs the number of defining exit status of previous command. (Command Prompt.) |
| %PROCESSOR\_IDENTIFIER% | Outputs processor identifier. |
| %PROCESSOR\_LEVEL% | Outputs processor level. |
| %PROCESSOR\_REVISION% | Outputs processor revision. |
| %NUMBER\_OF\_PROCESSORS% | Outputs the number of physical and virtual cores. |
| %RANDOM% | Outputs random number from 0 through 32767. |
| %OS% | Windows\_NT |

Although you can use environment variables to access certain locations within Windows 10 quickly, you’ll typically use these variables when building a script or an application.

Keep in mind that some of the variables mentioned are not location-specific, including `%COMPUTERNAME%`, `%PATHEXT%`, `%PROMPT%`, `%USERDOMAIN%`, `%USERNAME%`.

While this guide is focused on Windows 10, it’s important to note that these variables will also work on Windows 8.1, Windows 7, Windows Vista, and [Windows 11](https://pureinfotech.com/tag/windows-11).

You can always view all the environment variables available on your device using the `Get-ChildItem Env: | Sort Name PowerShell` command.

We may earn commission for purchases using our links to help keep offering the free content. [Privacy policy info](https://pureinfotech.com/privacy-policy/#affiliate-page-part "Privacy Policy").

All content on this site is provided with no warranties, express or implied. **Use any information at your own risk**. Always backup of your device and files before making any changes. [Privacy policy info](https://pureinfotech.com/privacy-policy "Privacy Policy").
