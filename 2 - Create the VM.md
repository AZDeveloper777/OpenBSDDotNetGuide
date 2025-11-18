1. Create the virtual hard drive image
```
cd ~/vms/testvm
vmctl create -s 60G ubuntu24.qcow2 
```
2. Create an entry in /etc/vm.conf for this VM
```
vm "ubuntu24" {
        memory 8G
        disable #change to enable if you want this vm to start when vmm starts
        disk /home/yourusername/vms/ubuntu24.qcow2
        local interface
        cdrom /home/yourusername/isos/ubuntu24.iso
        boot device cdrom
}
```
