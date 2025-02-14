

**PC0** 192.168.1.2

**PC1** 192.168.1.3

**PC2** 192.168.2.2

**PC3** 192.168.2.3

**PC4** 192.168.3.2

**PC5** 192.168.3.3



**Router 0(Left)**
```enable
hostname Left
configure terminal
interface GigabitEthernet0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet0/0
ip address 1.0.0.1 255.255.255.252
no shutdown
exit
router rip
version 2
no auto-summary
network 192.168.1.0
network 1.0.0.0
passive-interface GigabitEthernet0/1
exit
```



**Router 1(Middle)** 
```enable
hostname Middle
configure terminal
interface GigabitEthernet0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet0/0
ip address 1.0.0.2 255.255.255.252
no shutdown
exit
router rip
version 2
no auto-summary
network 192.168.1.0
network 1.0.0.0
passive-interface GigabitEthernet0/1
exit
```

**PC0** 
```
192.168.1.2
192.168.1.1
255.255.255.0
```
**PC1**
```
192.168.1.3
192.168.1.1
255.255.255.0
```
**PC3** 
```
192.168.2.2
192.168.2.1
255.255.255.0
```
**PC1**
```
192.168.2.3
192.168.2.1
255.255.255.0
```


**PC0**
```
C:\>ping 192.168.2.2

Pinging 192.168.2.2 with 32 bytes of data:

Reply from 192.168.2.2: bytes=32 time<1ms TTL=126
Reply from 192.168.2.2: bytes=32 time<1ms TTL=126
Reply from 192.168.2.2: bytes=32 time<1ms TTL=126
Reply from 192.168.2.2: bytes=32 time<1ms TTL=126

Ping statistics for 192.168.2.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

**PC3** 
```C:\>tracert 192.168.1.3

Tracing route to 192.168.1.3 over a maximum of 30 hops: 

  1   2 ms      0 ms      0 ms      192.168.2.1
  2   0 ms      0 ms      0 ms      1.0.0.1
  3   *         0 ms      0 ms      192.168.1.3

Trace complete.
```

**Adding Router**
**Adding PC4 and PC5**    

**Router 1(Right)**
```enable
hostname Right
configure terminal
int g0/2
ip address 2.0.0.1 255.255.255.252
no shutdown
exit
```

```
router rip
network 2.0.0.0
exit
```
**Router 2**
```
enable
configure terminal
interface GigabitEthernet0/2
no shutdown
ip address 2.0.0.2 255.255.255.252
exit
```

```
interface GigabitEthernet0/1
ip address 192.168.3.1 255.255.255.0
no shutdown
exit
```
```
router rip
version 2
no auto-summary
network 2.0.0.0
network 192.168.3.0
passive-interface GigabitEthernet0/1
exit
```

**PC4**
```
192.168.3.2
192.168.3.1
255.255.255.0
```

**PC5**
```
192.168.3.3
192.168.3.1
255.255.255.0
```

**Test**
```
C:\>ping 192.168.1.2

Pinging 192.168.1.2 with 32 bytes of data:

Reply from 192.168.1.2: bytes=32 time<1ms TTL=125
Reply from 192.168.1.2: bytes=32 time<1ms TTL=125
Reply from 192.168.1.2: bytes=32 time<1ms TTL=125
Reply from 192.168.1.2: bytes=32 time<1ms TTL=125

Ping statistics for 192.168.1.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

**PC4**
```
C:\>tracert 192.168.2.3

Tracing route to 192.168.2.3 over a maximum of 30 hops: 

  1   0 ms      0 ms      18 ms     192.168.3.1
  2   0 ms      0 ms      0 ms      2.0.0.1
  3   0 ms      0 ms      0 ms      192.168.2.3

Trace complete.

C:\>tracert 192.168.1.2

Tracing route to 192.168.1.2 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.3.1
  2   0 ms      0 ms      0 ms      2.0.0.1
  3   0 ms      0 ms      0 ms      1.0.0.1
  4   0 ms      0 ms      0 ms      192.168.1.2

Trace complete.
```
