# 19CN406-EX-13 SIMULATION OF OSPF USING OPNET SIMULATOR
# AIM:
To write a ns2 program for implementing multicasting routing protocol.
# ALGORITHM:
Step 1: start the program.
Step 2: declare the global variables ns for creating a new simulator. 
Step 3: set the color for packets.
Step 4: open the network animator file in the name of file2 in the write mode. Step 5: open 
the trace file in the name of file 1 in the write mode.
Step 6: set the multicast routing protocol to transfer the packets in network. Step 7: create the 
multicast capable no of nodes.
Step 8: create the duplex-link between the nodes including the delay time,bandwidth and 
dropping queue mechanism.
Step 9: give the position for the links between the nodes. 
Step 10: set a udp connection for source node.
Step 11: set the destination node ,port and random false for the source and destination files. 
Step 12: setup a traffic generator CBR for the source and destination files.
Step 13: down the connection between any nodes at a particular time.
Step 14: create the receive agent for joining and leaving if the nodes in the group. 
Step 15: define the finish procedure.
Step 16: in the definition of the finish procedure declare the global variables. 
Step 17: close the trace file and namefile and execute the network animation file. 
Step 18: at the particular time call the finish procedure
Step 19: stop the program.
PROGRAM:
# Create scheduler
#Create an event scheduler wit multicast turned on 
set ns [new Simulator -multicast on]
#$ns multicast #Turn on Tracing
set tf [open output.tr w]
$ns trace-all $tf
# Turn on nam Tracing set fd [open mcast.nam w] $ns namtrace-all $fd
# Create nodes
set n0 [$ns node]
set n1 [$ns node] 
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node] 
set n5 [$ns node] 
set n6 [$ns node] 
set n7 [$ns node] 
# Create links
$ns duplex-link $n0 $n2 1.5Mb 10ms DropTail
$ns duplex-link $n1 $n2 1.5Mb 10ms DropTail
$ns duplex-link $n2 $n3 1.5Mb 10ms DropTail
$ns duplex-link $n3 $n4 1.5Mb 10ms DropTail
$ns duplex-link $n3 $n7 1.5Mb 10ms DropTail
$ns duplex-link $n4 $n5 1.5Mb 10ms DropTail
$ns duplex-link $n4 $n6 1.5Mb 10ms DropTail
# Routing protocol: say distance vector #Protocols: CtrMcast, DM, ST, BST set mproto 
DM
set mrthandle [$ns mrtproto $mproto {}]
# Allocate group addresses
set group1 [Node allocaddr] set group2 [Node allocaddr]
# UDP Transport agent for the traffic source set udp0 [new Agent/UDP]
$ns attach-agent $n0 $udp0 $udp0 set dst_addr_ $group1 $udp0 set dst_port_ 0 set cbr1 [new 
Application/Traffic/CBR] $cbr1 attach-agent $udp0
# Transport agent for the traffic source
set udp1 [new Agent/UDP]
$ns attach-agent $n1 $udp1
$udp1 set dst_addr_ $group2
$udp1 set dst_port_ 0
set cbr2 [new Application/Traffic/CBR]
$cbr2 attach-agent $udp1
# Create receiver
set rcvr1 [new Agent/Null]
$ns attach-agent $n5 $rcvr1
$ns at 1.0 "$n5 join-group $rcvr1 $group1" set rcvr2 [new Agent/Null]
$ns attach-agent $n6 $rcvr2
$ns at 1.5 "$n6 join-group $rcvr2 $group1" set rcvr3 [new Agent/Null]
$ns attach-agent $n7 $rcvr3
$ns at 2.0 "$n7 join-group $rcvr3 $group1" set rcvr4 [new Agent/Null]
$ns attach-agent $n5 $rcvr1
$ns at 2.5 "$n5 join-group $rcvr4 $group2" set rcvr5 [new Agent/Null]
$ns attach-agent $n6 $rcvr2
$ns at 3.0 "$n6 join-group $rcvr5 $group2" set rcvr6 [new Agent/Null]
$ns attach-agent $n7 $rcvr3
$ns at 3.5 "$n7 join-group $rcvr6 $group2"
$ns at 4.0 "$n5 leave-group $rcvr1 $group1"
$ns at 4.5 "$n6 leave-group $rcvr2 $group1"
$ns at 5.0 "$n7 leave-group $rcvr3 $group1"
$ns at 5.5 "$n5 leave-group $rcvr4 $group2"
$ns at 6.0 "$n6 leave-group $rcvr5 $group2"
$ns at 6.5 "$n7 leave-group $rcvr6 $group2"
# Schedule events
$ns at 0.5 "$cbr1 start"
$ns at 9.5 "$cbr1 stop"
$ns at 0.5 "$cbr2 start"
$ns at 9.5 "$cbr2 stop"
#post-processing
REG NO:
$ns at 10.0 "finish" proc finish {} { global ns tf
$ns flush-trace close $tf
exec nam mcast.nam & exit 0
}
# For nam
#Colors for packets from two mcast groups
$ns color 10 red
$ns color 11 green
$ns color 30 purple
$ns color 31 green
# Manual layout: order of the link is significant! 
#$ns duplex-link-op $n0 $n1 orient right
#$ns duplex-link-op $n0 $n2 orient right-up
#$ns duplex-link-op $n0 $n3 orient right-down 
# Show queue on simplex link n0->n1 
#$ns duplex-link-op $n2 $n3 queuePos 0.5
# Group 0 source $udp0 set fid_ 10 $n0 color red
$n0 label "Source 1"
# Group 1 source
$udp1 set fid_ 11
$n1 color green
$n1 label "Source 2"
$n5 label "Receiver 1"
$n5 color blue
$n6 label "Receiver 2"
$n6 color blue
$n7 label "Receiver 3"
$n7 color blue
#$n2 add-mark m0 red 
#$n2 delete-mark m0" 
# Animation rate
$ns set-animation-rate 3.0ms
$ns run

