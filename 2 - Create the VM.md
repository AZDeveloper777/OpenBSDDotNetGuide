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
3. Backup your existing pf.conf
```
cp /etc/pf.conf /etc/pf.original
```
4. Replace the contents of your /etc/pf.conf with this to allow traffic to/from your VM and give it DNS:
```
dns_server="8.8.8.8"  
ext_if="egress"  
local_net="100.64.0.0/10"
set skip on lo
pass out on $ext_if from $local_net to any flags S/SA keep state
match out on egress from 100.64.0.0/10 to any nat-to (egress)
pass in proto { udp tcp } from 100.64.0.0/10 to any port domain rdr-to $dns_server port domain
```
