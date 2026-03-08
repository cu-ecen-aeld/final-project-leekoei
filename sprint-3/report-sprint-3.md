# Report for the final project sprint 3

In this report, the following DUTs (device under test) are defined:

* DUT1 = Raspberry Pi 4B unit running the Buildrooot generated Linux image
* DUT2 = Raspberry Pi 5 unit running the Raspberry Pi OS 64-bit

## 1. Drivertest on DUT1

From DUT1, since scp is not supported, create a drivertest.sh file and copy/paste the contents from 
https://github.com/cu-ecen-aeld/assignment-autotest/blob/83e5dfa52a482c76980e69e1f231c32a03c68910/test/assignment9-buildroot/drivertest.sh

```
# vi drivertest.sh
# chmod +x drivertest.sh 
```

Run the drivertest.sh script from DUT1:
```
# ./drivertest.sh 
The output below should show write 1 with first 2 bytes missing
ite1
write2
write3
write4
write5
write6
write7
write8
write9
write10
The output below should show the 9 from write 9 followed by write10 only
9
write10
```

The test is successful.

## 2. Socket test between DUT1 and DUT2

On DUT1, verify that the aesdsocket binary is running:
```
# ps | grep aesdsocket
  220 root     /usr/bin/aesdsocket
```
Verify an IP address is assigned through DHCP:
```
# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq qlen 1000
    link/ether dc:a6:32:88:5d:b8 brd ff:ff:ff:ff:ff:ff
    inet 192.168.68.92/22 brd 192.168.71.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fd48:8310:b66c:b259:dea6:32ff:fe88:5db8/64 scope global dynamic flags 100 
       valid_lft 1692sec preferred_lft 1692sec
    inet6 fe80::dea6:32ff:fe88:5db8/64 scope link 
       valid_lft forever preferred_lft forever
```

On DUT2, clone the project https://github.com/cu-ecen-aeld/assignment-5-leekoei.

Make sure DUT2 can ssh to DUT1 with root/root.

From DUT2, run the socket test script:
```
$ ./assignment-autotest/test/assignment9-buildroot/sockettest.sh -t 192.168.68.92 -p 9000
Testing target 192.168.68.92 on port 9000
Sending ioc seekto command for offset 0,2
rite1
swrite2
swrite3
swrite4
swrite5
swrite6
swrite7
swrite8
swrite9
swrite10
Sending ioc seekto command for offset 8,6
9
swrite10
Test passed
```
The socket test was successfully completed. A screenshot is also attached:

![socket-test-screenshot](https://github.com/cu-ecen-aeld/final-project-leekoei/blob/sprint-3/pics/rpi4b-test.png)

The DoD of Sprint 3 is met, and the final project is completed.
