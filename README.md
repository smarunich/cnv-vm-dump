# cnv-vm-dump
Utility to perform "virsh dump" for Openshift CNV guest workloads. It creates a dump file containing the core of the guest virtual machine so that it can be analyzed, for example by the crash utility.
```
[root@ocp-stg-bastion cnv-vm-dump]# ./cnv-vm-dump.sh --help
Usage: script <vm> [-n <namespace>]  --pause|--dump [full|memory]|--list|--copy [filename]|--unpause
```

## HowTo

### Step 1 - Pause the target VM
```
[root@bastion cnv-vm-dump]# oc get vmi
NAME                    AGE   PHASE     IP             NODENAME
smarunich-cnv-win10-1   46m   Running   10.128.0.1   worker-0.redhat.com
```
### Step 2 - Perform the target VM dump
There are two options available:
* full - to perform complete VM dump (to include memory)
* memory - to perform memory VM dump only
```

```
### Step 3 - List available VM dumps for copy

### Step 4 - Copy the target VM dump

### Step 5 - Unpause the target VM