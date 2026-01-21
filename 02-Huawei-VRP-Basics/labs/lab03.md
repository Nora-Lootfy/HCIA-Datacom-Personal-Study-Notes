# VRP Basic Configuration Commands

## Requirement description

* Connect the router and PC. Assign the IP addresses shown in the figure to the router and PC.
* Allow other employees of the company to use the password huawei123 to remotely log in to the router through the PC. Allow them to view configurations but disable them from modifying configurations.
* Save current configurations and name the configuration file huawei.zip. Configure this file as the configuration file for the next startup.


<p align="center">
  <img src="../images/lab3.png" width="450">
</p>

## Lab Solution

> [!Note]
> I have used the default router in ensp

```vrp
<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]interface Ethernet 0/0/0
[Huawei-Ethernet0/0/0]ip address 192.168.1.1 24
[Huawei-Ethernet0/0/0]quit
[Huawei]user-interface vty 0 4
[Huawei-ui-vty0-4]set authentication password cipher test
[Huawei-ui-vty0-4]user privilege level 1
[Huawei-ui-vty0-4]quit
[Huawei]quit
<Huawei>save huawei.zip
Are you sure to save the configuration to flash:/huawei.zip?[Y/N]:y
Now saving the current configuration to the slot 17.
Jan 21 2026 18:41:22-08:00 Huawei %%01CFM/4/SAVE_FILE(l)[1]:When deciding whethe
r to save the configuration to the file flash:/huawei.zip, the user chose Y.
Save the configuration successfully.
<Huawei>startup saved-configuration huawei.zip
Info: Succeeded in setting the configuration for booting system.
<Huawei>display startup
MainBoard: 
  Configured startup system software:        NULL
  Startup system software:                   NULL
  Next startup system software:              NULL
  Startup saved-configuration file:          NULL
  Next startup saved-configuration file:     flash:/huawei.zip
  Startup paf file:                          NULL
  Next startup paf file:                     NULL
  Startup license file:                      NULL
  Next startup license file:                 NULL
  Startup patch package:                     NULL
  Next startup patch package:                NULL
```