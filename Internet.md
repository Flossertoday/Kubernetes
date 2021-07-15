# How to ping host from virtualBox
This really took me a few days. None of the guidances I found worked. 
A few worked partially though. 

###### Get Host IP
Assuming your host is runing Windows. 

    ipconfig

Record IPv4 Address and Subnet Mask. 

###### Manage virtual ethernet adapters
In VirtualBox, not in the virtualized system. 

File -> Host Network Manager(Ctrl + H)

If there isn't a Host-Only Ethernet Adapter, create one. 

Choose "Configure Adapter Manually". 

- Fill in an IPv4 Address which only differs with Host IPv4 in its last of 4 parts. 

        Host: 192.168.10.1
        Virtual: 192.168.10.2

Like that. 

- Fill in the same Network Mask as the Host. 

- In my case, DHCP Server is disabled. I don't know what will happen if it's enabled instead. 

- If nothing is going wrong, IPv6 stuffs under **Adapter** tab will be greyed out. As well as all four blanks under **DHCP** tab. 

System should be powered off to apply these changes. 

###### More configurations in the virtualized system. 

System: CentOS 8

Get into Settings -> Network. Create a new profile. 

The idea is that we will have 2 ethernet adapters which will have their own profiles. Adapater 1 will work for connecting to the internet. The other will work for connecting to the Host. 

Now you should have 2 adapters and 2 profiles. 

- First profile: 

  - Under Identity tab, choose a MAC Address to fix this profile on Adapter 1. 

  - Under IPv4 tab, choose **Automatic (DHCP)** for **IPv4 Method**. Then set DNS & Routes **Automatic** ON. Leave everything else blank. 

- The other profile:

  - Under Identity tab, choose the other MAC Address. 

  - Under IPv4 tab, choose **Manual** for **IPv4 Method**. Set **Addresses** -> **Address** to the IPv4 address that you want your virtual machine to have. The same IP that was decided above. Leave Netmask and Gateway empty. Set **DNS** & **Routes** **Automatic**. 

  - Leave everything else alone. 

Now you're mostly done. Make sure both ethernet adapters are connected. Then reboot. 

###### Check
Run in virtualized system. 

    ping www.baidu.com
    ping *Host IP*

Also try in Host. 

    ping www.baidu.com
    ping *Virtual IP*