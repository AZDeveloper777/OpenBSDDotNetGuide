This guide assumes you have OpenBSD 7.8 installed on physical hardware.  

1. Create the dirs we need and download Ubuntu Server 24.04.3 LTS from and put it in ~/home/isos
   ```
   mkdir -p ~/isos  
   mkdir -p ~/isos/build  
   mkdir -p ~/vms  
   mkdir -p ~/vms/testvm  
   ```
2. Get the tools we'll need:
   ```
   pkg_add xorriso nano
   ```
3. extract the iso's existing grub.cfg. This what we need to modify to make the installer accessible via the serial console; your only option with OpenBSD vmm.
   ```
   cd ~/isos  
   osirrox -indev ./ubuntu-24.04.3-live-server-amd64.iso -extract boot/grub/grub.cfg build/grub.cfg
   ```
4. Use nano to edit the grub.cfg you extracted into ~/isos/build. Replace the contents of the file with this:
   ```
   set timeout=5

   loadfont unicode

   set menu_color_normal=white/black
   set menu_color_highlight=black/light-gray

   menuentry "Try or Install Ubuntu Server" {
   	set gfxpayload=keep
   	linux	/casper/vmlinuz file=/cdrom/preseed/ubuntu-server.seed vga=normal console=tty0 console=ttyS0,115200n8 ---
   	initrd	/casper/initrd
   }
   fi
   ```
5. Now lets use the output from xorriso to give us the base from which we will make a shell script that we will use to create the new iso. This will take a few steps.
   ```
   cd ~/isos
   xorriso -indev ./ubuntu-24.04.3-live-server-amd64.iso -report_system_area cmd > myisobuilder.sh
   ```
6. Edit the script and remove everything BEFORE the line that starts with -volid and replace it with:
   ```
   xorriso -indev ./ubuntu-24.04.2-live-server-amd64.iso \  
   -outdev ./ubuntu24.iso \  
   -map build/grub.cfg boot/grub/grub.cfg \  
   ```
   and then add " \ " at the end of each line in the file except the very last line.
7. Make it executable
   ```
   chmod +x myisobuilder.sh
   ```
8. Run the script
   ```
   myisobuilder.sh
   ```
9. You should now see a file with the name ubuntu24.iso in ~/isos
   
   
