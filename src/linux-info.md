Gathering running Linux instance info
===

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

TBD
