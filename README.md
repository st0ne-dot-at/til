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
    vpn_name='myvpn'

    if ping  -c 1 www.google.at >/dev/null 2>&1; then
        if ! nmcli connection show --active | grep "^${vpn_name}.*" >/dev/null 2>&1; then
            nmcli connection up ${vpn_name}
        fi
    fi

## 7.11.2018 add github routes to vpn connection 

    nmcli connection modify vpn +ipv4.routes 140.82.118.3/32
    nmcli connection modify vpn +ipv4.routes 140.82.118.4/32

## 7.11.2018 chrome reload proxy.pac file

    chrome://net-internals/#proxy -> Proxy -> Re-apply settings

## 8.11.2018 pstree back to init

    pstree -sp $$

## 14.11.2018 excract rpm content

    rpm2cpio myrpmfile.rpm | cpio -idmv

## 30.11.2018 git remove local branch

    git branch -d dev

## 30.11.2018 git remove remote branch

    git push --delete origin dev

## 12.12.2018 bash check string length

    [ -z "${test}" ] && echo 'not zero length'

## 13.12.2018 git diff file between branches

    git diff branch-a..branch-b  -- <file_to_diff>

## 9.1.2019 postgres csv export

    \copy (SELECT * FROM persons) to '/tmp/persons_client.csv' with csv

## 30.8.2019 pythonic read file line by line

    filepath = 'file.txt'
    with open(filepath) as f:
        for cnt, line in enumerate(f):
            print("Line {}: {}".format(cnt, line))

## 18.11.2019 rpm download only with dependencies

    yumdownloader --destdir . --resolve varnish.x86_64

## 21.11.2019 bash

    #default value
    $ echo ${not_set_var:-default_value}
    default_value

    #default value + set var
    $ echo ${not_set_var:=default_value}
    default_value
    $ echo $not_set_var 
    default_value

    #error on if unset
    $ echo ${not_set_var:?var is not set}
    -bash: not_set_var: var is not set

    #alternate value
    $ x=9999
    $ echo ${x:+1}
    1

    #substring
    $ x='abcdefg'
    $ echo ${x:1}
    bcdefg
    $ echo ${x:1:3}
    bcd

    #array
    $ x=(a b c d)
    $ echo ${x[*]}  # values
    a b c d
    $ echo ${!x[*]} # indexes
    0 1 2 3
    $ echo ${#array[*]} #size
    4

    #variable lenth
    $ x='a234'
    $ echo ${#x}
    4

    #remove prefix pattern
    $ x='P1234_99'
    $ echo ${x#P*_}
    99

    #remove suffix pattern
    $ x='nix_1asdf.txt'
    $ echo ${x%_*.txt}
    nix

    #substitute
    $ x='nix_1asdf.txt'
    $ echo ${x/nix/qwert}
    qwert_1asdf.txt
    $ echo ${x/1*/qwert}
    nix_qwert

    #modify case
    $ x=st0nE
    $ echo ${x^}
    St0nE
    $ echo ${x^^}
    ST0NE
    $ echo ${x,}
    st0nE
    $ echo ${x,,}
    st0ne

    #arithmetic
    $ x=1
    $ echo $((${x}+2))
    3

## 22.11.2019 list namespaces

    lsns


## 27.11.2019 find processes using swap

    grep -i vmswap /proc/*/status | sort -n -k2

## 27.11.2019 journald recover disk space

    journalctl --disk-usage
    journalctl --vacuum-size=10M

## 25.2.2020 socat multicast

    # send multicast
    echo "from: $(hostname)" | socat STDIO UDP4-DATAGRAM:239.255.200.192:4131,ip-multicast-ttl=8
    # receive multicast
    socat UDP4-RECVFROM:4131,ip-add-membership=239.255.200.192:$(ip route get 8.8.8.8 | head -n1 | awk '{print $7}'),fork,reuseaddr - 

## 16.9.2020 control Sony Android TV with curl
    # get methods
    curl -X POST http://10.0.1.218/sony/system -H "X-Auth-PSK: 1234" -d '{"id": 20, "method": "getMethodTypes", "version": "1.0", "params": [""]}' 
    curl -X POST http://10.0.1.218/sony/appControl -H "X-Auth-PSK: 1234" -d '{"id": 20, "method": "getMethodTypes", "version": "1.0", "params": [""]}' 
    # power off
    curl -X POST http://10.0.1.218/sony/system -H "X-Auth-PSK: 1234" -d '{"id": 20, "method": "setPowerStatus", "version": "1.0", "params": [{"status": false}]}'
    # power on
    curl -X POST http://10.0.1.218/sony/system -H "X-Auth-PSK: 1234" -d '{"id": 20, "method": "setPowerStatus", "version": "1.0", "params": [{"status": true}]}'
    # start netflix
    curl -X POST http://10.0.1.218/sony/appControl -H "X-Auth-PSK: 1234" -d '{"id": 20, "method": "setActiveApp", "version": "1.0", "params": [{"uri": "com.sony.dtv.com.netflix.ninja.com.netflix.ninja.MainActivity"}]}'

## 22.10.2020 compile shell script to c
   check out https://github.com/neurobin/shc
   
## 22.10.2020 source local .sshrc profile file on remote
   check out https://github.com/danrabinowitz/sshrc 
   
## 15.11.2023 cisc <-> linux lacp + vlan

    # delete all interface on linux machine
    # add bond interface
    nmcli connection add type bond con-name bond ifname bond mode 802.3ad ipv4.addresses 10.0.1.106/24 ipv4.gateway 10.0.1.1 ipv4.dns 10.0.1.1 bond.options mode=802.3ad,miimon=100,lacp_rate=fast,xmit_hash_policy=layer2+3
    
    # add slave interfaces
    nmcli connection  add type  bond-slave ifname enp0s20f0u6 con-name <slave-ineterface-1> master bond
    nmcli connection  add type  bond-slave ifname enp0s20f0u6 con-name <slave-ineterface-2> master bond
    
    # cisco config
    conf t
    interface GigabitEthernet1/0/11
    channel-protocol lacp
    channel-group 6 mode active
    exit
    interface GigabitEthernet1/0/12
    channel-protocol lacp
    channel-group 6 mode active
    exit
    
    interface Port-channel6
    description port-channel 6 desc
    switchport access vlan 37
    switchport mode access
    spanning-tree portfast
    spanning-tree link-type point-to-point
    exit



