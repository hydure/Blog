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
2.	*Discusses topology* Here is the entire network we will be creating. It may look very daunting and complex, but the process of creating this network is incredibly simple. This network is essentially only a VM, or virtual machine, with a firewall. The NIC, or network interface card, allows us to connect our VM to the Internet, and the public IP address allows us to reach our Minecraft server from the Internet, both of these components as well as the subnet and virtual network are created when we create the VM. The security group can also, and will, be created when we create our VM. So, again, this may look complex but is a rather easy and straightforward process.
3.	When the Azure portal opens, click on “Create a resource” located at the top of the left-hand side’s navigation panel. From there type “Ubuntu Server 18.04 VM” or click on its icon found under the “Popular” section as shown here. 
4.	Now we arrive at the “Blade”, “Create a virtual machine” where we will configure the VM that will host our Minecraft server. Choose a subscription that covers how much money you plan on spending to run your Minecraft server. After choosing the proper subscription, we need to choose which Resource group, which is the container that holds every component of our Minecraft network, we will use. Since we do not have one yet, click “Create new” and then create a name for this new Resource Group.
5.	Now we need to give the VM a name. I’d recommend making the name something obvious, like MinecraftServerVM. Then choose a region that you are in to ensure the lowest possible latency between you and your server. Keep the availability options and image options the same, and then choose what size you’d like your server to be. I recommend the B2s as the absolute minimum size for your server.
6.	Next choose which authentication type you would like to use to login to your VM. In this video, I will choose the “Password” option, but I recommend creating an SSH public key and choosing the “SSH public key” option to ensure that you are using the highest levels of security for your virtual machine.