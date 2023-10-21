---
layout: post
title:  "Setting up an Apache Server with Oracle Cloud and a custom domain"
date:   2023-10-20 13:03:30 -0600
categories: IT&C 210A update
---
## Introduction
Welcome to this comprehensive guide that will walk you through the process of setting up an Apache Server with Oracle Cloud and configuring it with a custom domain. 
# Apache Server
An Apache HTTP Server, often referred to simply as Apache, is one of the most widely used web server software in the world. It is an open-source web server designed to serve web content securely, efficiently, and reliably. Apache provides a robust and flexible platform for hosting websites, web applications, and various services. We will explore how to set up and configure Apache to meet your specific hosting needs.
# Oracle Cloud
Oracle Cloud is a comprehensive cloud computing platform offered by Oracle Corporation. It provides a wide range of cloud services, including computing, storage, database, networking, and more. Oracle Cloud is known for its high performance, scalability, and security, making it an excellent choice for hosting web applications and websites. We will guide you through the process of setting up an Oracle Cloud instance, ensuring a stable and secure environment for your Apache Server.
# Custom Domain
A custom domain is a crucial element for any online presence. It allows you to personalize your website's URL to reflect your brand or content. By using a custom domain, you can establish a professional and memorable web address that aligns with your objectives. We will show you how to acquire and configure a custom domain to make your Apache Server easily accessible under your chosen web address. We will be using an open source project called duckdns.


Throughout this guide, we will delve into the step-by-step procedures to bring these elements together. By the end of this tutorial, you will have a fully functional Apache Server hosted on Oracle Cloud, accessible via your custom domain. Whether you're a seasoned developer or a novice looking to establish an online presence, this guide will provide you with the knowledge and skills necessary to get your web server up and running efficiently. Let's embark on this journey to create a secure, reliable, and customized web hosting environment.

Feel free to jump around if you need to skip a step - you could also use your own domain system, but for this guide I will stick with duckdns.

# Sign up for an Always Free Account on Oracle Cloud
If you don't have an account, you'll need to sign up for an "always free" account in Oracle Cloud. Go to https://www.oracle.com/cloud/free/ and click on "Start for free".

![Oracle Sign Up](/assets/signup0.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Fill out the form, make sure that you are a human and not a robot and then click on "Verify my email".

![Oracle Sign Up](/assets/signup1.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Continue with the sign up process by filling out the form and clicking on "Continue" at the end of the page.
Make sure to click on Individual under Customer Type, and then to pick a name that you will remember for your Cloud Account Name.

![Oracle Sign Up](/assets/signup2.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

You will then be asked to insert your Address Information and to verify your phone number.

![Oracle Sign Up](/assets/signup3.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

You then will have to "Add a payment verification method". It's important to notice that "You won't be charged unless you elect to upgrade the account". You won't be charged unless you want more out of your account, so don't worry about it.

![Oracle Sign Up](/assets/signup4.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Once you've added a payment verification method, you are done with the signup process! The only thing left to do is to agree to the terms and conditions and to click on "Start my free trial".

![Oracle Sign Up](/assets/signup5.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

It might take a while for your account to be created. Just be patient! Once it will be finilized you will receive an email saying that everything went correctly.

# Create a Virtual Machine Instance
Once you have your account, you will need to create a Virtual Machine Instance. To do so, go to the Oracle Cloud Console and, under "Lunch Resources" click on "Create a VM instance".

![Oracle VM Creation](/assets/vm0.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Give your instance a name and leave root as the compartment. Choose any "Availability domain", it doesn't really matter for us.

![Oracle VM Creation](/assets/vm1.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Select "Edit" under "Image and shape" and change to whatever OS you prefer. Make sure that it says "Always Free-eligible" as you select the OS. In my case I am going to use Canonical Ubuntu 20.04. Under "Shape" I strongly advise you to select "VM.Standard.A1.Flex" as it is the only one that is free. Also ander "Shape name" you can select a maximum number of 4 OCPUs and a maximum number of 24 GB or RAM. I would suggest to play with it - as long as it sayas "Always Free-eligible" you are good to go.

![Oracle VM Creation](/assets/vm2.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Next, we'll need to create a Virtual Cloud Network for our machine to work on. Under Primary VNIC infromation, and under "Primary network", select "Create Virtual Cloud Network". Give it a name and leave the rest as default. 

![Oracle VM Creation](/assets/vm3.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Under "Primary VNIC IP addresses", leave the default options as shown below.

![Oracle VM Creation](/assets/vm4.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

The next step will be very very important. We'll need to associate a SSH key pair to the virtual machine. If you know what you're doing, go on and upload your public key. If not, leave "Generate a key pair for me" and select "Save private key". It's fondamental that you keep this key safe in your computer or wherever you may like.

![Oracle VM Creation](/assets/vm5.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Uncheck "Use in-transit encryption" (if you want), under "Boot volume" and finally click on "Create".

You will be redirected to a page with the details of the VM and for a couple of minute you will see the status as "Provisioning". Once it's done, you will see the status as "Running".

![Oracle VM Creation](/assets/vm6.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

