# Unifi Controller on AWS

The purpose of this document is to describe the process of setting up the Unifi controller in a self hosted cloud. This is by no means a replacement for the Unifi Cloud Access, simply an alternative to the publicly available cloud access that will support multiple sites cost effectively, all while remaining controller over my data/security. At my sites I have the Ubiquiti EdgeRouter X installed, and the plan is to build aWireGuard tunnel between the device and my AWS instance. In theory, this should allow me to pass my internal network traffic directly to the controller itself, and allow for easier adoption for my devices as well. This should also allow me to limit the number of ports I have to allow on the external interface for the controller to be able to connect to my devices. 

## AWS Hosting

For the hosting provider I choose to use an AWS EC2 t3a.micro server with 30 GB of storage. In an attempt to keep this project cost effective, I chose to use an AWS Spot Instance with Persistence over a Dedicated Instance. At the time of writing this particular Spot instance cost $0.0028 in my selected region, versus $0.0104 for the Dedicated Instance. 

## Access

### The Theory - In Practice

The whole purpose for this, is I would like to be able to able to access my controller while on the go from my phone or laptop. The theory for setting up a dual access connection using both HTTPS and WiredGaurd is so only the web interface is open to the public, and all other required ports are access via the encrypted WireGuard Connection. For the web interface, a proxy will lie between the controller and the external interface to allow for a second layer of authentication, as well as applying security best practices for SSL and TLS. 

### Dynamic DNS

With this server being a spot instance as well as attempting to be cost effective, I will not be investing in a static IP, I will use DDNS instead. For example, to access the web interface, I'll go to 'controller.example.com' to access the unifi controller interface, and 'tunnel.example.com' to access the wireguard tunnel. Since the address will be dynamic I will have to have a way to update both entries in the event of an address change. The two options I've considered for doing this is using DDclient, or using a bash script to collect the public IP, and then publish it to my DNS provider. 

### Top Level Domain

In an attempt to add to the obscurity, I choose to literally 'blackhole' the top level domain. I'm not super worried about people finding my public IPs as my DNS provider allows me to obscure the origin IP. 

### WireGuard

On top of running the controller on AWS the plan is to also run a WireGuard server as well. The reason behind running WireGuard is to create an encrypted tunnel to pass all AP to Controller Communication

### The Theory - Conclusion

Having a game plan is nice, but as we all know, things don't always follow the theory. Due to this fact, what follows will be the actual process/procedure/outcome of this experiment. 

#### Potentially Useful links

https://community.ui.com/questions/UniFi-Installation-Scripts-or-UniFi-Easy-Update-Script-or-UniFi-Lets-Encrypt-or-Ubuntu-16-04-18-04-/ccbc7530-dd61-40a7-82ec-22b17f027776

https://gist.github.com/vidia/fbef2ee643b23848d8b24211d5860b78

https://mediatemple.net/community/products/developer/204405534/install-nginx-on-ubuntu

https://www.linode.com/docs/web-servers/nginx/enable-tls-on-nginx-for-https-connections/

https://www.stevejenkins.com/blog/2016/05/diy-cloud-hosting-ubiquiti-ubnt-unifi-controller/

https://www.reddit.com/r/WireGuard/comments/bjx5cy/using_wireguard_with_an_aws_vpc_private_subnet/

https://www.reddit.com/r/linux4noobs/comments/a99k43/trying_to_update_ip_for_multiple_sub_domains_on/

https://gist.github.com/Tras2/cba88201b17d765ec065ccbedfb16d9a

https://community.cloudflare.com/t/lets-encrypt-and-cloudflare-how-to-set/66442/4

https://christine.website/blog/site-to-site-wireguard-part-1-2019-04-02windows 

DYNDNS
    
    https://community.ui.com/questions/How-to-set-up-IP-updater-for-openDNS/b7e07e65-1863-4cc6-a9df-0e70de86c541#answer/f0cab9da-80a7-4ec3-a2a3-1cf2c7bb387e

    https://community.ui.com/questions/Change-WAN-DNS-Server/041bbac7-6de0-44a7-a5ca-165128e4333d#answer/dae84067-53e6-4cd4-b7f5-bbcaefa10223