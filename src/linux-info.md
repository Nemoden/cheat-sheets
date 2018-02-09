Gathering running Linux instance's info
===

In order to find out most information about Linux instance one can use wide variety of command and tools, this cheat-sheet is my compilation of what you can do to discover a Linux instance you eigher `ssh'ed` to or using as your workspace.

## Hardware

If lshw in available, it's very handy to get information about what is the instance running on

```
lshw -short
```

If have root access:

```
dmidecode | less
```

Sample output:

```
H/W path    Device  Class      Description
==========================================
                    system     HVM domU
/0                  bus        Motherboard
/0/0                memory     96KiB BIOS
/0/401              processor  Intel(R) Xeon(R) CPU E5-2676 v3 @ 2.40GHz
/0/1000             memory     1GiB System Memory
/0/1000/0           memory     1GiB DIMM RAM
/0/100              bridge     440FX - 82441FX PMC [Natoma]
/0/100/1            bridge     82371SB PIIX3 ISA [Natoma/Triton II]
/0/100/1.1          storage    82371SB PIIX3 IDE [Natoma/Triton II]
/0/100/1.3          bridge     82371AB/EB/MB PIIX4 ACPI
/0/100/2            display    GD 5446
/0/100/3            generic    Xen Platform Device
/1          eth0    network    Ethernet interface
```

## Instance type and OS

```
lsb_release -a
```

or

```
cat /etc/*release*
```

The above will use `glob` to `cat` all `/etc` files containing `release` as filename part.

Reading from `/proc`

```
cat /proc/version
```

Using `uname`

```
uname
```

All information about instance type

```
uname -a
```

All uname flags:

```
-a  print all information, in the following order, except omit -p and -i if unknown:

-s print the kernel name

-n print the network node hostname

-r print the kernel release

-v print the kernel version

-m print the machine hardware name

-p print the processor type or "unknown"

-i print the hardware platform or "unknown"

-o print the operating system
```

## CPU information

```
lscpu
```

## Find out if this instance is virtualized

Using `lscpu`

```
lscpu | grep -i 'Hyper\|Virt'
```

Reading from `/proc`

```
cat /proc/cpuinfo
```

Using `dmidecode` (needs root access)

```
# dmidecode -t system | grep 'Manufacturer\|Product'
	Manufacturer: Xen
	Product Name: HVM domU
```

## Memory

Using `free` (if `-h` option is available, the output will be a bit nicier to read). Showing memory in megabytes

```
free -mt
```

Reading from `/proc`

```
cat /proc/meminfo
```

## Listing block devices

```
lsblk
```

or

```
lsblk -a
```

## HDD

Using `df`

```
df -h
```

Using `fdisk`

```
fdisk -l
```

## Networking

TBD

## Processes and ports

Basic overview of what services are running and which ports they are listenning (ommit `n` option if you don't want names to be resolved to IPs).

```
netstat -tulpn
```

`t` option is for `tcp`, `u` is for `udp`. Lising only unix sockets can be done with:

```
netstat -lpn
```

For better grasp of what you can do with `netstat` visit the command's `man` page (`man netstat`)

To find out which processes occupy certain port run (you might need to be `root` or being able to run commands using `sudo`)

```
lsof -i :<port>
```

Sample output

```
lsof -i :80
COMMAND   PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   14755 www-data    6u  IPv4 496315      0t0  TCP *:http (LISTEN)
nginx   14755 www-data    7u  IPv6 496316      0t0  TCP *:http (LISTEN)
nginx   24727     root    6u  IPv4 496315      0t0  TCP *:http (LISTEN)
nginx   24727     root    7u  IPv6 496316      0t0  TCP *:http (LISTEN)
```

### What processes are crrently rnning

By current user

```
ps ux
```

All users

```
ps aux
```

Adding `f` (`--forest`) option will give you a nice treeview of processes running along with child processes they have spawned

```
ps axjf
```

You can use `top` (or `htop` if available) command to see processes running and how much resources those consume

```
top
```

To quit hit `q`

You can use `pstree` for a brief yet good overview of what is going on on the system

```
$ pstree
systemd-+-accounts-daemon-+-{gdbus}
        | `-{gmain}
        | -acpid
        | -2*[agetty]
        | -atd
        | -cron
        | -dbus-daemon
        | -dhclient
        | -2*[iscsid]
        | -lvmetad
        | -lxcfs-+-{lxcfs) S 1 906 9
        | `-9*[{lxcfs}]
        | -mdadm
        | -memcached---5*[{memcached}]
        | -mysqld---28*[{mysqld}]
        | -nginx---nginx
        | -php-fpm7.0---3*[php-fpm7.0]
        | -polkitd-+-{gdbus}
        | `-{gmain}
        | -redis-server---2*[{redis-server}]
        | -rsyslogd-+-{in:imklog}
        |                                                   | -{in:imuxsock}
        | `-{rs:main Q:Reg}
        | -snapd---6*[{snapd}]
        | -sshd-+-sshd---sshd---bash---sudo---bash---pstree
        | `-sshd
        | -systemd---(sd-pam)
        | -systemd-journal
        | -systemd-logind
        | -systemd-timesyn---{sd-resolve}
        | -systemd-udevd
        `-uuidd
```

To find out which services are currently running you can use `service` comman:

```
$ service --status-all
 [ + ]  acpid
 [ + ]  apparmor
 [ + ]  apport
 [ + ]  atd
 [ - ]  bootmisc.sh
 [ - ]  checkfs.sh
 [ - ]  checkroot-bootclean.sh
 [ - ]  checkroot.sh
 [ + ]  console-setup
 [ + ]  cron
 [ - ]  cryptdisks
 [ - ]  cryptdisks-early
 [ + ]  dbus
 [ + ]  grub-common
 [ - ]  hostname.sh
 [ - ]  hwclock.sh
 [ + ]  irqbalance
 [ + ]  iscsid
 [ - ]  keyboard-setup.dpkg-bak
 [ - ]  killprocs
 [ + ]  kmod
 [ - ]  lvm2
 [ + ]  lvm2-lvmetad
 [ + ]  lvm2-lvmpolld
 [ + ]  lxcfs
 [ - ]  lxd
 [ + ]  mdadm
 [ - ]  mdadm-waitidle
 [ + ]  memcached
 [ - ]  mountall-bootclean.sh
 [ - ]  mountall.sh
 [ - ]  mountdevsubfs.sh
 [ - ]  mountkernfs.sh
 [ - ]  mountnfs-bootclean.sh
 [ - ]  mountnfs.sh
 [ + ]  mysql
 [ + ]  networking
 [ + ]  nginx
 [ + ]  ondemand
 [ + ]  open-iscsi
 [ - ]  open-vm-tools
 [ + ]  php7.0-fpm
 [ - ]  plymouth
 [ - ]  plymouth-log
 [ - ]  procps
 [ + ]  rc.local
 [ + ]  redis-server
 [ + ]  resolvconf
 [ - ]  rsync
 [ + ]  rsyslog
 [ - ]  screen-cleanup
 [ - ]  sendsigs
 [ + ]  ssh
 [ + ]  udev
 [ + ]  ufw
 [ - ]  umountfs
 [ - ]  umountnfs.sh
 [ - ]  umountroot
 [ + ]  unattended-upgrades
 [ + ]  urandom
 [ + ]  uuidd
```

Legend: `+` - service is on, `-` - service is off, `?` - unknown status

## Network information

To find out configuration

```
ifconfig
```

To find out hosts which are "seen" from current machine

```
arp -a
```
