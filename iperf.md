How to Use Iperf to Test Network Performance in Linux

by admin

The iperf is a tool used for testing the network performance between two systems. The iperf application provides more metrics for a networks’ performance. The iperf application is not installed by default, but it is provided by most distributions’ package manager.

For CentOS/RHEL/Fedora Systems use the yum, to install the iperf package.
```
# yum install iperf
```
iperf has the notion of a “client” and “server” for testing network throughput between two systems.

The following example sets a large send and receive buffer size to maximise throughput, and performs a test for 60 seconds which should be long enough to fully exercise a network.
Server

On the server system, iperf is told to listen for a client connection:
```
server # iperf3 -i 10 -s
```
Here,
-i – the interval to provide periodic bandwidth updates
-s – listen as a server

See man iperf3 for more information on specific command line switches.
Client

On the client system, iperf is told to connect to the listening server via hostname or IP address:
```
client # iperf3 -i 10 -w 1M -t 60 -c [server hostname or ip address]
```
Here,
-i – the interval to provide periodic bandwidth updates
-w – the socket buffer size (which affects the TCP Window). The buffer size is also set on the server by this client command
-t – the time to run the test in seconds
-c – connect to a listening server at…

See man iperf3 for more information on specific command line switches.
Test Results

Both the client and server report their results once the test is complete:
Server
```
server # iperf3 -i 10 -s
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
Accepted connection from 10.0.0.2, port 22216
[  5] local 10.0.0.1 port 5201 connected to 10.0.0.2 port 22218
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-10.00  sec  17.5 GBytes  15.0 Gbits/sec                  
[  5]  10.00-20.00  sec  17.6 GBytes  15.2 Gbits/sec                  
[  5]  20.00-30.00  sec  18.4 GBytes  15.8 Gbits/sec                  
[  5]  30.00-40.00  sec  18.0 GBytes  15.5 Gbits/sec                  
[  5]  40.00-50.00  sec  17.5 GBytes  15.1 Gbits/sec                  
[  5]  50.00-60.00  sec  18.1 GBytes  15.5 Gbits/sec                  
[  5]  60.00-60.04  sec  82.2 MBytes  17.3 Gbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-60.04  sec  0.00 Bytes    0.00 bits/sec                  sender
[  5]   0.00-60.04  sec   107 GBytes  15.3 Gbits/sec                  receiver

Client

client # iperf3 -i 10 -w 1M -t 60 -c 10.0.0.1
Connecting to host 10.0.0.1, port 5201
[  4] local 10.0.0.2 port 22218 connected to 10.0.0.1 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-10.00  sec  17.6 GBytes  15.1 Gbits/sec    0   6.01 MBytes       
[  4]  10.00-20.00  sec  17.6 GBytes  15.1 Gbits/sec    0   6.01 MBytes       
[  4]  20.00-30.00  sec  18.4 GBytes  15.8 Gbits/sec    0   6.01 MBytes       
[  4]  30.00-40.00  sec  18.0 GBytes  15.5 Gbits/sec    0   6.01 MBytes       
[  4]  40.00-50.00  sec  17.5 GBytes  15.1 Gbits/sec    0   6.01 MBytes       
[  4]  50.00-60.00  sec  18.1 GBytes  15.5 Gbits/sec    0   6.01 MBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-60.00  sec   107 GBytes  15.4 Gbits/sec    0             sender
[  4]   0.00-60.00  sec   107 GBytes  15.4 Gbits/sec                  receiver
```
Reading the Results

Between these two systems, we could achieve a bandwidth of 15.4 gigabit per second or approximately 1835 MiB (mebibyte) per second.

Here,
Interval means the test interval, the defualt value is 10 seconds and displays as “0.0-10.0 sec”
Transfer means how much data is transfered between the two nodes in the process of testing.
Bandwidth is the performance indicator which we use iperf testing for.
Note: The server listens on TCP port 5201 by default. This port will need to be allowed through any firewalls present. The port used can be changed with the -p commandline option.
Testing network performance with UDP protocol

The default iperf uses TCP protocol for testing as shown above. Add option “-u” to use UDP protocol for performance testing.

1. First step is to start the server.
```
# iperf -s -u
```
```
------------------------------------------------------------
Server listening on UDP port 5001
Receiving 1470 byte datagrams
UDP buffer size:   124 KByte (default)
------------------------------------------------------------

[  3] local 1.1.1.1 port 5001 connected with 1.1.1.2 port 51598
[ ID] Interval       Transfer     Bandwidth       Jitter   Lost/Total Datagrams
[  3]  0.0-10.0 sec  1.25 MBytes  1.05 Mbits/sec  0.004 ms    0/  893 (0%)

2. The command on client side is as shown below.

# iperf -u -c server
------------------------------------------------------------
Client connecting to 1.1.1.1, UDP port 5001
Sending 1470 byte datagrams
UDP buffer size:   124 KByte (default)
------------------------------------------------------------
[  3] local 1.1.1.2 port 51598 connected with 1.1.1.1 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  1.25 MBytes  1.05 Mbits/sec
[  3] Sent 893 datagrams
[  3] Server Report:
[ ID] Interval       Transfer     Bandwidth       Jitter   Lost/Total Datagrams
[  3]  0.0-10.0 sec  1.25 MBytes  1.05 Mbits/sec  0.004 ms    0/  893 (0%)
````
Changing the default bandwidth in UDP mode

In the UDP mode, the client uses default bandwidth (1 Mbit/sec) to send the packets to the server, so we can see the Bandwidth of UDP testing result is 1.05 Mbits/sec. In order to obtain the best network bandwidth, we can add option “-b value” to increase the sending bandwidth:
```
# iperf -u -c server -b 1000M
------------------------------------------------------------
Client connecting to 1.1.1.1, UDP port 5001
Sending 1470 byte datagrams
UDP buffer size:   124 KByte (default)
------------------------------------------------------------
[  3] local 1.1.1.2 port 58097 connected with 1.1.1.1 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  1.10 GBytes    948 Mbits/sec
[  3] Sent 806540 datagrams
[  3] Server Report:
[ ID] Interval       Transfer     Bandwidth       Jitter   Lost/Total Datagrams
[  3]  0.0-10.0 sec  1.08 GBytes    929 Mbits/sec  0.010 ms 16203/806540 (2%)

Measuring the Maximum Transfer Size (MTU)

The -m option instructs iperf to also find the Maximum Transfer Size (MTU):

# iperf -mc 192.168.10.12
------------------------------------------------------------
Client connecting to 192.168.10.12, TCP port 5001
TCP window size: 19.3 KByte (default)
------------------------------------------------------------
[  3] local 192.168.10.13 port 46558 connected with 192.168.10.12 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec   113 MBytes  94.7 Mbits/sec
[  3] MSS size 1448 bytes (MTU 1500 bytes, ethernet)
```

Link: 

https://www.thegeekdiary.com/how-to-use-iperf-to-test-network-performance-in-linux/
