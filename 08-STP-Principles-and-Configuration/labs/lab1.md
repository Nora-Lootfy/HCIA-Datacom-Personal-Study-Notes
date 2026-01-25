# Basic STP Configuration


## Scenario
- Deploy STP on the three switches to eliminate Layer 2 loops on the network.
- Configure SW1 as the root bridge and block GE0/0/22 on SW3


## Lab solution

### SW1 configuration
```
[SW1]stp mode ?
  mstp  Multiple Spanning Tree Protocol (MSTP) mode
  rstp  Rapid Spanning Tree Protocol (RSTP) mode
  stp   Spanning Tree Protocol (STP) mode

[SW1]stp mode stp
Info: This operation may take a few seconds. Please wait for a moment...done.
[SW1]stp enable
[SW1]stp priority 0
[SW1]display stp brief
 MSTID  Port                        Role  STP State     Protection
   0    GigabitEthernet0/0/1        DESI  FORWARDING      NONE
   0    GigabitEthernet0/0/2        DESI  FORWARDING      NONE
```

### SW2 configuration
```
[SW2]stp mode stp
Info: This operation may take a few seconds. Please wait for a moment...done.
[SW2]stp enable
[SW2]stp priority 4096
```