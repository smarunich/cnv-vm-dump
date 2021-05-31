# cnv-vm-dump
Utility to perform "virsh dump" for Openshift CNV guest workloads. It creates a dump file containing the core of the guest virtual machine so that it can be analyzed, for example by the crash utility.
```
[root@bastion cnv-vm-dump]# ./cnv-vm-dump.sh --help
Usage: script <vm> [-n <namespace>]  --pause|--dump [full|memory]|--capture_mode [dump|snapshot]|--list|--copy [filename]|--unpause
```

## Requirements

- oc
- virtctl

## HowTo

### Step 1 - Pause the target VM within the defined namespace
```
[root@ocp-stg-bastion cnv-vm-dump]# oc get vmi -n forensics-cnv
NAME                    AGE   PHASE     IP             NODENAME
forensics-cnv-win10-0   16m   Running   10.128.2.207   worker-0.redhat.com
[root@ocp-stg-bastion cnv-vm-dump]# ./cnv-vm-dump.sh -n forensics-cnv forensics-cnv-win10-0  --pause
VMI forensics-cnv-win10-0 was scheduled to pause
```
### Step 2 - Option A using virsh dump - Perform the target VM dump (Default)
There are two options available:
* full - to perform complete VM dump (to include memory)
* memory - to perform memory VM dump only
```
[root@bastion cnv-vm-dump]#  ./cnv-vm-dump.sh -n forensics-cnv forensics-cnv-win10-0 --dump memory --capture_mode dump
Dump: [100 %]
Domain forensics-cnv_forensics-cnv-win10-0 dumped to /var/run/kubevirt/external/forensics-cnv_forensics-cnv-win10-0/forensics-cnv_forensics-cnv-win10-0-20210331-145428.memory.dump
```
### Step 2 - Option B using virsh snapshot - Perform the target VM snapshot
There are two options available:
* full - to perform complete VM dump (to include memory) *** NOT SUPPORTED *** 
* memory - to perform memory VM dump only
```
[root@bastion cnv-vm-dump]#  ./cnv-vm-dump.sh -n forensics-cnv forensics-cnv-win10-0 --dump memory --capture_mode snapshot
Dump: [100 %]
Domain forensics-cnv_forensics-cnv-win10-0 dumped to /var/run/kubevirt/external/forensics-cnv_forensics-cnv-win10-0/forensics-cnv_forensics-cnv-win10-0-20210331-145428.memory.snapshot
```
### Step 3 - List available VM dumps for copy
```
[root@bastion cnv-vm-dump]#  ./cnv-vm-dump.sh -n forensics-cnv forensics-cnv-win10-0 --list
drwxr-xr-x. 2 root root   77 May  5 14:16 .
drwxr-xr-x. 3 root root   49 May  5 14:16 ..
-rw-------. 1 root root 4.1G May  5 14:16 forensics-cnv_forensics-cnv-win10-0-20210505-101625.memory.dump
```
### Step 4 - Copy the target VM dump
```
[root@bastion cnv-vm-dump]#  ./cnv-vm-dump.sh -n forensics-cnv forensics-cnv-win10-0 --copy forensics-cnv_forensics-cnv-win10-0-20210331-145428.memory.dump
Defaulting container name to compute.
tar: Removing leading `/' from member names
[root@bastion cnv-vm-dump]#  ls
forensics-cnv_forensics-cnv-win10-0-20210331-145428.memory.dump
```
### Step 5 - Unpause the target VM
```
[root@bastion cnv-vm-dump]#  ./cnv-vm-dump.sh -n forensics-cnv forensics-cnv-win10-0 --unpause
VMI forensics-cnv-win10-0 was scheduled to unpause
```
