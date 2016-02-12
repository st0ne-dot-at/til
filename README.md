# til

## 12.2.2016 snoop lacep

    tcpdump -i eth0 -c1 -s0 -vvv ether dst host 01:80:c2:00:00:02

## 11.2.2016 fast server reboot

    
    kexec -l /boot/vmlinuz-$(uname -r) \
                   --initrd=/boot/initramfs-$(uname -r).img \
                   --reuse-cmdline
    kexec -e


## 11.2.2016 dump cdp-neighbour packets

    tcpdump -i eth0 -v -s 0 -c 1 'ether[20:2] == 0x2000'

## 11.2.2016 change keyboard layout on console

    localectl set-keymap --no-convert keymap us
