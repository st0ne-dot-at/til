# til

## 29.3.2016 paste in vim
    :set noautoindent
    :set paste

## 10.3.2016 see what a rpm provides

    rpm -q --provides openpgm-5.2.122-2.el6.x86_64

## 19.2.2016 journalctl show logs per unit

    journalctl -u <unit-name>

## 19.2.2016 journalctl show full line

    journalctl --no-pager | less

## 12.2.2016 snoop lacp

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
