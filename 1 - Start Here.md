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
4. Edit the grub.cfg you extracted into ~/isos/build. Replace the contents of the file with this:
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
