Static IP addresses increase network security and control connections if they are more than required. Having a static IP address will give users consistent availability and improve reliability as well as provide you with a fixed geolocation. Static IPs are necessary for devices that need constant access and best for businesses who host their own websites.

-> Let us configure static ip, for which firstly go to the oracle virtual box, then to the settings.

-> Now, Go to the Network tab in settings, if we enable NAT (Network Address Translation) as Network Adapter, by default outgoing connections are allowed but incoming connections are blocked. In  Advanced settings, click on port forwarding. 

>Start -> Oracle VirtualBox -> VM -> Network -> Enable NAT as Network Adapter,  -> Advanced Settings => Port Forwarding -> Add port -> Ok

-> The drawback with NAT network adapter is that, for every application we need to do port forwarding for providing required application's port number. For example, to ssh login into any machine, 22 port number need to be allowed as mentioned below.

-> Hence change the network adapter from NAT interface to the Bridged Adapter.
Instead of getting IP addresses from the oracle virtual box, we get IP addresses directly from the router itself. In Advanced settings, Promiscuous mode is set to allow all. Bridged Adapter will get an IP address as same as the base machine gets directly from the router.

For configuring the static IP address for docker host machine, open the following file `/etc/netplan/00-installer-config.yaml` file through any of the file editors. We have chosen a nano editor. 

Initially the file will have some data as shown below. We need to append the file with necessary configuration to maintain the ip static.
![[Pasted image 20240705150813.png]]

While most of the network connected devices receive their IP address dynamically through DHCP,  we need to set dhcp to false to provide the static ip address to the virtual machine, then add routes such as gateway, and also add global nameservers. Now save and quit the file by clicking on ctrl+X.
![[Pasted image 20240705150838.png]]

To see the changes on the machine,we need to apply the changes using the command netplan apply.
`netplan apply`

ip a command will show the ip addresses of the host machine. Static ip given in the above file has to be reflected here. When reboot or shutdown the virtual machine, the ip should not change as we are using the static ip here.
![[Pasted image 20240705150933.png]]

`reboot` command will reboot the system. Here it is used for verifying the static ip behavior when the connection is lost or the virtual machine is restarted.

IP address has not changed as we have configured static ip.  
![[Pasted image 20240705151005.png]]









