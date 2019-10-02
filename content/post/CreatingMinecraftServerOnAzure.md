---
title: "Creating a Minecraft Server on Azure Cloud"
date: 2019-09-29T10:54:28-04:00
draft: false
toc: false
comments: false
categories:
- Cloud Development
tags:
- Minecraft
- Azure
---

This post explains how I created a Minecraft server using Azure cloud because I could not find any working template for such a case.
<!--more-->
First off, I'm sorry for not blogging in a while, there have been many changes in my life that I will discuss in another blog post. Let's dive in to the topic of this post for now.

NOTE: This is to create a Minecraft server for the Java, NOT WINDOWS 10, version of Minecraft using Azure cloud.

1.	Create an Azure cloud account.

    a.	Select free account from the top left of the Azure homepage.

    b.	Click the green button on the blue banner labelled, “Start for free”.

    c.	Sign into your Microsoft account. If you do not have a Microsoft account, then click “Create Microsoft account.

    d.	After signing in, and creating your account if you didn’t already have one, give Azure a couple of seconds to load and then complete this form. Even though this is a free trial account, you will need to enter your credit/debit card details when Azure charges you for its services. The Minecraft server we will be making costs about $31 per month if you keep it running for the whole month, but there are far cheaper options – I just like to have up to 20 people playing on my Minecraft server at the same time. Agree to the terms and conditions then click “Sign up” to finish this process.

    e.	Once your subscription is ready for you, click the green button to start using Azure.

2. Below is the entire network we will be creating. It may look very daunting and complex, but the process of creating this network is incredibly simple. This network is essentially only a VM, or virtual machine, with a firewall. The NIC, or network interface card, allows us to connect our VM to the Internet, and the public IP address allows us to reach our Minecraft server from the Internet, both of these components as well as the subnet and virtual network are created when we create the VM. The security group can also, and will, be created when we create our VM. So, again, this may look complex but is a rather easy and straightforward process. ![Server topology..](/miscPics/serverTopology.png)

3.	When the Azure portal opens, click on “Create a resource” located at the top of the left-hand side’s navigation panel. From there type “Ubuntu Server 18.04 VM” or click on its icon found under the “Popular” section as shown here. 

4.	Now we arrive at the “Blade”, “Create a virtual machine” where we will configure the VM that will host our Minecraft server. Choose a subscription that covers how much money you plan on spending to run your Minecraft server. After choosing the proper subscription, we need to choose which Resource group, which is the container that holds every component of our Minecraft network, we will use. Since we do not have one yet, click “Create new” and then create a name for this new Resource Group.

<!--adsense-->

5.	Now we need to give the VM a name. I’d recommend making the name something obvious, like MinecraftServerVM. Then choose a region that you are in to ensure the lowest possible latency between you and your server. Keep the availability options and image options the same, and then choose what size you’d like your server to be. I recommend the B2s as the absolute minimum size for your server.

6.	Next choose which authentication type you would like to use to login to your VM. In this video, I will choose the “Password” option, but I recommend creating an SSH public key and choosing the “SSH public key” option to ensure that you are using the highest levels of security for your virtual machine.

7.	The last thing on this page we need to do is to open up the SSH port for remote login so we can access our server remotely. Under “Inbound Port Rules”, select “Allow selected ports” and then select the SSH port.

8.	Now go to the Networking tab. Here we will attach our VM to a virtual network, assign a public IP address for it, and create a network security group to keep our network safe from unauthorized users. This may all sound dauting, but it is incredibly simple.

<!--adsense-->

9.	A virtual network is a remote computer network, meaning your virtual computers, aka VMs, can attach themselves to this network in order to communicate with one another and computers on networks that are connected with your virtual network. Since we do not have a virtual network yet, we click “Create new” to create a new virtual network. Give the network a name, I would recommend MinecraftVNet, and click “OK”. Next, go to the Public IP form field and click “Create New”. Assign the public IP address a name, I chose to go for MinecraftPublicIP, and then click “OK”.

10.	Now we go to the next field, entitled, “NIC network security group”, to configure a security group for our network. This will prevent a lot of unwanted and unauthorized traffic to enter and leave our network, as well as allow us to grant express permissions to allow people to play Minecraft on our server. Click the “Advanced” button, and click “Create New” under the dropdown box that just appeared to create a new network security group.

11.	A little panel opens up on the left side of our screen. Click “add an inbound rule” to add the only other rule we need to add, an Allow rule so we can have people including ourselves be able to connect to and play Minecraft on our Minecraft server. Select “Service Tag” for Source, “Internet” for “Source service tag”, “25565” for “Destination port ranges”, “TCP” as the “Protocol”, “Priority” to 101, and the name to something obvious like “AllowMinecraft”. Finally, click “Add” to create the inbound rule. Click “OK” to finish creating your network security group.

12.	Click “Review + create” to create your VM and all the components for your network. You should have the same resources discussed at the beginning of this video: a VM, a NSG, a VNet, a subnet, a NIC, and a public IP address. Now we just need to configure a DNS name for our VM so we can connect to our Minecraft server from our Minecraft login clients.

<!--adsense-->

13.	Click on “All Resources” on the left-hand navigation panel and select your public IP address. A new blade should appear. Click on “Configuration” on the blade’s left-hand navigation panel. Choose “Dynamic” under the Assignment section and then provide a DNS name under the “DNS name label” field. Once this is complete, you can now login to your VM and download the Minecraft server remotely.

14. Log into your remote VM and follow the instructions listed on [Linuxize](https://linuxize.com/post/how-to-install-minecraft-server-on-ubuntu-18-04/) to successfully install a Minecraft server onto your remote VM.

I hope this has proven useful to some of you and saves you the headache of following old YouTube videos or using a currently-broken Azure template. Thank you for reading!

<!--adsense-->