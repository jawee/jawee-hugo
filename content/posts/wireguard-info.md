---
title: "Wireguard Info"
date: 2020-09-25T13:36:45Z
draft: false
---
Got a good post when trying to figure out how to get a wireguard tunnel going on the selfhosted.show discord from user ndonegan, hopefully this will help the next time I try setting it up.

His post follows:

Remember, TCP is two way. Take two hosts, `host_1` which has `eth0` as `192.168.0.1` on `192.168.0.0/24` and `host_2` which has `eth0` as `192.168.100.1` on `192.168.100.0/24`

Each of them can communicate to the local /24 as it knows that eth0 is the best local route.

Now, if you add Wireguard, it's the equivilent of dragging a cable between the two hosts on another physical interface on their own little network. So lets say `host_1` has `10.0.0.1/24` on `wg0` and `host_2` has `10.0.0.2/24` on `wg0`.

`host_1` knows that to hit `10.0.0.2` it can go over `wg0` and likewise `host_2` knows that to hit `10.0.0.1` is can go over `wg0`. You can see this using `ip route get $ip`

The thing is, `host_1` doesn't have the first clue how to hit anything on the `192.168.100.0/24` network as it doesn't have that in it's route table. It's going to attempt to send it over it's default gateway! The same situation exists for `host_2` trying to hit `192.168.0.0/24`

The easiest way to fix this is to use static routes. So, for example, on `host_1`, you do `ip route add 192.168.100.0/24 via 10.0.0.2 dev wg0` and on `host_2` you do `ip route add 192.168.1.0/24 via 10.0.0.1 dev wg0`.

What this means is that if you initiate a connection from `host_1` to `192.168.100.1`, it's going to look at the route table, see that routing that packet to `10.0.0.2` via `wg0` is the best route to get to that network. 

There's a slight fun gotcha: On the other side, it's actually going to see the connection as coming from `10.0.0.1` so it will route the traffic back to that address. The reason it will see the traffic as coming from `10.0.0.1`? Assuming you have the above setup and run `ip route get 192.168.100.1` on host_1 you will see:
```bash
$ ip route get 192.168.100.1
192.168.100.1 via 10.0.0.2 dev wg0 src 10.0.0.1
```
So, any traffic going to `192.168.100.1` will appear to come from `10.0.0.1`.
If `192.168.0.1` is the gateway for the `192.168.0.0/24` and another host, say `192.168.0.2` attempts to connect to `host_2` on `192.168.100.1` it's source ip will be `192.168.0.2`. This means when the packet reachs `host_2`, it's going to look at it's route table to see what's the best path back to `192.168.0.2`. With the static route added earlier, it's going to see to send back the traffic it has to go via `10.0.0.1` on `wg0` and `host_1` will then forward the traffic onto `192.168.0.2`.
