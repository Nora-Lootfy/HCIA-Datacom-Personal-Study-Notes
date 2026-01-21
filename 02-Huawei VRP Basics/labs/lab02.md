# File Operations

## Requirement description

* Rename the huawei.txt file save.zip.
* Make a copy for the save.zip file and name the copy file.txt.
* Move the file.txt file to the dhcp directory.
* Delete the file.txt file.
* Restore the deleted file file.txt


## Lab Solution

> [!Note]
> Before solving this lab, I have to create a file on the Router, so I enter system view and rename the router then run the command `save huawei.zip` then rename the file to be huawei.txt

```vrp
<RTA>rename huawei.txt save.zip
Rename flash:/huawei.txt to flash:/save.zip ?[Y/N]:y
Info: Rename file flash:/huawei.txt to flash:/save.zip ......Done.
<RTA>copy save.zip file.txt
Copy flash:/save.zip to flash:/file.txt?[Y/N]:y

100%  complete
Info: Copied file flash:/save.zip to flash:/file.txt...Done.
<RTA>delete file.txt 
Delete flash:/file.txt?[Y/N]:y
Info: Deleting file flash:/file.txt...succeeded.
<RTA>undelete file.txt 
Undelete flash:/file.txt?[Y/N]:y
%Undeleted file flash:/file.txt.
<RTA>
```