# File Query Commands and Directory Operations

## Requirement description

* Check information about files and directories in the current directory of a router named RTA.
* Create a directory named test, and then delete the directory.


## Lab Solution

```vrp
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname RTA
[RTA]quit
<RTA>dir
Directory of flash:/

  Idx  Attr     Size(Byte)  Date        Time       FileName 
    0  drw-              -  Aug 07 2015 13:51:14   src
    1  drw-              -  Jan 21 2026 18:20:18   pmdata
    2  drw-              -  Jan 21 2026 18:20:23   dhcp
    3  -rw-             28  Jan 21 2026 18:20:24   private-data.txt

32,004 KB total (31,995 KB free)

<RTA>mkdir test
Info: Create directory flash:/test......Done.
<RTA>dir
Directory of flash:/

  Idx  Attr     Size(Byte)  Date        Time       FileName 
    0  drw-              -  Aug 07 2015 13:51:14   src
    1  drw-              -  Jan 21 2026 18:20:18   pmdata
    2  drw-              -  Jan 21 2026 18:20:23   dhcp
    3  -rw-             28  Jan 21 2026 18:20:24   private-data.txt
    4  drw-              -  Jan 21 2026 18:21:25   test

32,004 KB total (31,994 KB free)

<RTA>rmdir test/ 
Remove directory flash:/test?[Y/N]:y
%Removing directory flash:/test...Done!
<RTA>dir
Directory of flash:/

  Idx  Attr     Size(Byte)  Date        Time       FileName 
    0  drw-              -  Aug 07 2015 13:51:14   src
    1  drw-              -  Jan 21 2026 18:20:18   pmdata
    2  drw-              -  Jan 21 2026 18:20:23   dhcp
    3  -rw-             28  Jan 21 2026 18:20:24   private-data.txt

32,004 KB total (31,995 KB free)

```

