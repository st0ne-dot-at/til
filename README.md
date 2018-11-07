# til
## 1.12.2016 xargs echo server

    nc -l -u 9999 -k -c 'xargs -n1 echo'

## 20.4.2016 get apk from android device

    adb shell pm list packages
    adb shell pm path com.example.someapp
    adb pull /data/app/com.example.someapp-2.apk
    
## 9.4.2016 ping scan with nmap

    nmap -sn 10.0.1.0/24

## 22.3.2016 reload udev rules

    udevadm control --reload-rules

## 22.3.2016 udev trigger script on usb storage insert

    KERNEL=="sd?1", SUBSYSTEMS=="usb", DRIVERS=="usb-storage", ACTION=="add", RUN+="/woswasi.sh"

## 22.3.2016 get udev information of device

    udevadm info -a /dev/sdb1

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

## 7.11.2018 auto start vpn with NetworkManager

    #!/bin/bash
    #file: /etc/NetworkManger/dispatcher.d/30-autovpn.sh
    vpn_name='vpn'
    parent_dev='enp0s25'
    
    if [ "$1" == $parent_dev -a "$2" == 'up' ]; then
        nmcli connection up $vpn_name
    fi
