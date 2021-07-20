## Issue
I've had too many issues in this process. I'm pretty sure that this article will be a mess. 

But everything came from this ErrorMsg:

- Lens: 

    No connection could be made because the target machine actively refused it. 


## Background
CentOS8 in VirtualBox under Windows 10
Adapter 1 NAT
Adapter 2 Host-Only

## Solution
First of all, turn down **iptables.service**. 

    systemctl stop iptables
    systemctl disable iptables

Disable Adapter 2. Switch adapter 1 from NAT to Bridged. 

After that, Host and VM cannot ping each other anymore, but it worths. 

Reset Kubeadm, then initialize kubedam again, but with slight difference this time. 

    kubeadm init --image-repository=registry.aliyuncs.com/google_containers --pod-network-cidr=10.244.0.0/16  --ignore-preflight-errors=Swap

**--apiserver-advertise-address** is no longer used. 

###### Kubeconfig
Do not manually c/p **kubeconfig** to lens, as it might cause issues when switching between Linux/Unix. 

Instead, cp **kubeconfig** file to ~/.kube

Open Lens. This Cluster could be found in Catalog -> Clusters. Connect. 

You should be done after that. 

## Other commands
###### Clear firewall rules
Delete all chains

    iptables -F

Accept all

    iptables -P INPUT ACCEPT
    iptables -P OUTPUT ACCEPT
    iptables -P FORWARD ACCEPT

Get current firewall rules

    iptables -L

Save firewall rules edits

    service iptables save


