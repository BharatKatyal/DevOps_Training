# CKAD Exam Curriculum

## Application Design and Build (20%)
- Define, build and modify container images
- Choose and use the right workload resource (Deployment, DaemonSet, CronJob, etc.)
- Understand multi-container Pod design patterns (e.g. sidecar, init and others)
- Utilize persistent and ephemeral volumes

## Application Deployment (20%)
- Use Kubernetes primitives to implement common deployment strategies (e.g. blue/green or canary)
- Understand Deployments and how to perform rolling updates
- Use the Helm package manager to deploy existing packages
- Kustomize

## Application Observability and Maintenance (15%)
- Understand API deprecations
- Implement probes and health checks
- Use built-in CLI tools to monitor Kubernetes applications
- Utilize container logs
- Debugging in Kubernetes

## Application Environment, Configuration and Security (25%)
- Discover and use resources that extend Kubernetes (CRD, Operators)
- Understand authentication, authorization and admission control
- Understand requests, limits, quotas
- Understand ConfigMaps
- Define resource requirements
- Create & consume Secrets
- Understand ServiceAccounts
- Understand Application Security (SecurityContexts, Capabilities, etc.)

## Services and Networking (20%)
- Demonstrate basic understanding of NetworkPolicies
- Provide and troubleshoot access to applications via services
- Use Ingress rules to expose applications





Network and How Docker Uses Network Namespaces



Linux Networking Basics

Switches and routing
Default Gateway

DNS
- DNS Configuration on Linux
- CoreDNS Intro


ip link - shows interface on host
ip addr 192.168.1.11/24 dev eth0 - sets the ip address of the interface eth0 to 192.168.1.11 and the subnet mask of 24

route - displays systems routing table
ip route add 192.168.1.0/24 via 192.168.1.1 - adds a route to the routing table allowing routing traffic through the default gateway of 192.168.1.1


This setting governs if you can forward messages

cat /proc/sys/net/ipv4/ip_forward
0 - means you cannot forward messages
echo 1 > /proc/sys/net/ipv4/ip_forward
cat /proc/sys/net/ipv4/ip_forward
1 - means you can forward messages

However this is not permannet and will be reset on reboot
for that you need to edit
/etc/sysctl.conf
and add
net.ipv4.ip_forward=1
and then
sysctl -p
to reload the config


Key Commands
ip link - list and modify network interfaces
ip addr - list and modify ip addresses
ip addr add 192.168.1.11/24 dev eth0 - adds an ip address to the interface eth0 - Only valid for the session. if you want to make it permanent you need to edit the network config file for the interface which is at /etc/sysconfig/network-scripts/ifcfg-eth0 and add the IPADDR=192.168.1.11 and the NETMASK=255.255.255.0 and then restart the network service or the interface

ip route - list and modify routing table
route  - 
cat /proc/sys/net/ipv4/ip_forward -> 0
ip -s link - show interface statistics


ping 192.168.1.1  might be your db
but when you ping db it may not work untill you edit
/etc/hosts and add
192.168.1.1 db 

now ping db works

/etc/resolv.conf - contains the nameservers for the system
nameserver 8.8.8.8


/etc/nsswitch.conf - contains the name service switch configuration
This file controls the order in which name services are used for host name resolution. For example, it can specify whether to look up host names in /etc/hosts, DNS, or NIS in a particular order.

Public name servers
8.8.8.8
1.1.1.1


. root
com top level domain
google
subdomain - www, map, mail (mail.google.com)

/etc/resolve.conf - contains the search path for the system
search default.svc.cluster.local svc.cluster.local cluster.local
or
search mycompany.com prod.mycompany.com


Record Types
A Records
AAAA Record
CNAME Record - Name to Name Mapping

Ping may not always be the right tool
nsloopup - it does not consider the entries in local etc.host file. It looks up on your DNS server
dig - testing dns name resolution is the best tool

Port 53 Default port for a DNS Server 


--- Network namespaces ---
