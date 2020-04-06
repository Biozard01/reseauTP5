# TP 5

## I. Toplogie 1 - intro VLAN

### 2. Setup clients

- les admins se joignent entre eux

```cisco
admin1> ping 10.5.10.12
84 bytes from 10.5.10.12 icmp_seq=1 ttl=64 time=0.898 ms
84 bytes from 10.5.10.12 icmp_seq=2 ttl=64 time=1.407 ms
84 bytes from 10.5.10.12 icmp_seq=3 ttl=64 time=1.318 ms
^C
admin1>
```

```cisco
admin2> ping 10.5.10.11
84 bytes from 10.5.10.11 icmp_seq=1 ttl=64 time=2.182 ms
84 bytes from 10.5.10.11 icmp_seq=2 ttl=64 time=1.775 ms
^C
admin2>
```

- les guests se joignent entre eux

```cisco
guest1> ping 10.5.20.12
84 bytes from 10.5.20.12 icmp_seq=1 ttl=64 time=1.651 ms
84 bytes from 10.5.20.12 icmp_seq=2 ttl=64 time=1.664 ms
^C
guest1>
```

```cisco
guest2> ping 10.5.20.11
84 bytes from 10.5.20.11 icmp_seq=1 ttl=64 time=1.132 ms
84 bytes from 10.5.20.11 icmp_seq=2 ttl=64 time=1.428 ms
84 bytes from 10.5.20.11 icmp_seq=3 ttl=64 time=1.382 ms
^C
guest2>
```

### 3. Setup VLANs

- les ports vers les clients (admins et guests) sont en access

```cisco
IOU1#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et0/2, Et0/3, Et1/0
                                                Et1/1, Et1/2, Et1/3, Et2/0
                                                Et2/1, Et2/2, Et2/3, Et3/0
                                                Et3/1, Et3/2, Et3/3
10   admins                           active    Et0/1
20   guests                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
10   enet  100010     1500  -      -      -        -    -        0      0
20   enet  100020     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0
1003 tr    101003     1500  -      -      -        -    -        0      0
1004 fdnet 101004     1500  -      -      -        ieee -        0      0
1005 trnet 101005     1500  -      -      -        ibm  -        0      0
```

- les ports qui relient deux switches sont en trunk

```cisco
IOU1#show interface trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       1-4094

Port        Vlans allowed and active in management domain
Et0/0       1,10,20

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       1,10,20
IOU1#
```

- les guests peuvent toujours se ping, idem pour les admins

```cisco
admin1> ping 10.5.10.12
84 bytes from 10.5.10.12 icmp_seq=1 ttl=64 time=1.835 ms
84 bytes from 10.5.10.12 icmp_seq=2 ttl=64 time=1.648 ms
84 bytes from 10.5.10.12 icmp_seq=3 ttl=64 time=1.879 ms
^C
admin1>
```

```cisco
admin2> ping 10.5.10.11
84 bytes from 10.5.10.11 icmp_seq=1 ttl=64 time=1.196 ms
84 bytes from 10.5.10.11 icmp_seq=2 ttl=64 time=0.606 ms
84 bytes from 10.5.10.11 icmp_seq=3 ttl=64 time=0.617 ms
^C
admin2>
```

```cisco
guest1> ping 10.5.20.12
84 bytes from 10.5.20.12 icmp_seq=1 ttl=64 time=1.286 ms
84 bytes from 10.5.20.12 icmp_seq=2 ttl=64 time=1.508 ms
84 bytes from 10.5.20.12 icmp_seq=3 ttl=64 time=1.907 ms
^C
guest1>
```

```cisco
guest2> ping 10.5.20.11
84 bytes from 10.5.20.11 icmp_seq=1 ttl=64 time=1.460 ms
84 bytes from 10.5.20.11 icmp_seq=2 ttl=64 time=1.880 ms
84 bytes from 10.5.20.11 icmp_seq=3 ttl=64 time=2.304 ms
^C
guest2>
```

- montrer que si un des guests change d'IP vers une IP du réseau admins, il ne peut PAS joindre les autres admins

```cisco
guest1> ping 10.5.10.11
host (10.5.10.11) not reachable

guest1> ping 10.5.10.12
host (10.5.10.12) not reachable

guest1>
```

## II. Topologie 2 - VLAN, sous-interface, NAT
