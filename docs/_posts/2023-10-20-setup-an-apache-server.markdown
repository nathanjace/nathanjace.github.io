---
layout: post
title:  "Setting up an Apache Server with Oracle Cloud and a custom domain"
date:   2023-10-20 13:30:00 -0600
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
If you don't have an account, you'll need to sign up for an "always free" account in Oracle Cloud. Go to [https://www.oracle.com/cloud/free/] and click on "Start for free".

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

Once it's up and running, scroll down until you find "Primary VNIC" and mark down the Public IPv4 address. You will need this to connect to your VM through SSH.

![Oracle VM Creation](/assets/vm7.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

# Connect to your VM through SSH
To connect to the just created server we will need to use SSH. If you are on Windows, use PowerShell or WSL, if you're on MacOS or Linux, simply use your favorite terminal. 

First, we'll need to change the permissions of the private key we just downloaded. To do so, run the following command:

{% highlight ruby %}
```sh
chmod 400 <path_to_your_private_key>
```
{% endhighlight %}

Then, we'll need to connect to the VM. To do so, run the following command:

{% highlight ruby %}
```sh
ssh -i <path_to_your_private_key> ubuntu@<your_public_ipv4_address>
```
{% endhighlight %}

And that's it! You are in your VM! You should see something like this:

![SSH](/assets/ssh0.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

In this case I don't mind sharing my IPv4 address, since it's simply used for internal services I have running on my VM. 
You run the following command to get all caught up with the latest updates:

{% highlight ruby %}
```sh
sudo apt update && sudo apt upgrade -y
```
{% endhighlight %}

# Install Apache
Now that we are all caught up with the latest updates, we can install Apache. To do so, run the following command:

{% highlight ruby %}
```sh
sudo apt install apache2
```
{% endhighlight %}

You can check your server's state with `sudo service apache2 <option>`, where `<option>` can be `{start|stop|graceful-stop|restart|reload|force-reload|status}`. It's easy to understand what each command does.

Start your apache2 service and check its status with the following two commands:

{% highlight ruby %}
```sh
sudo service apache2 start
```
```sh
sudo service apache2 status
```
{% endhighlight %}

When you install and configure apache2, it will automatically open a port in your server (usually 80 for HTTP and 443 for HTTPS) from which you will access the webpage you want to host. If it doesn't, Ubuntu comes with a pretty nifty service: a firewall. To open the port 80 and 443, run the following command:

{% highlight ruby %}
```sh
sudo ufw enable
```
```sh
sudo ufw allow 80,443/tcp
```
```sh
sudo ufw reload
```
{% endhighlight %}

# Open Firewall and Security List Ports
IMPORTANT: If you don't do this, you won't be able to access your webpage from the outside world, even if you opened the ports on your ubuntu server.

Back to the Oracle Cloud Console, under your VM instance detail page you were in previously, scroll down again until you find "Primary VNIC". 

![Primary VNIC](/assets/vm7.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Click on public subnet-`<name of your subnet>` abd then click, under "Security Lists", "Default Security List for `<name of your subnet>`".

Since 80 and 443 are used all the times in networking, you might see them already there. If not, click on "Add Ingress Rules" and add them. Under "Source CIDR" you can put 0.0.0.0/0 to allow access from anywhere. Select TCP as the "IP Protocol". Under "Destination Port Range" you can put 80,443. Finally, click on "Add Ingress Rules".

![Security List](/assets/ports0.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

# Set up a domain name
For this guide we will use a pretty straighforward and easy method to assign a domain name to our server. We will use duckdns. [Duck DNS](https://www.duckdns.org/) is a free service which will point a DNS (sub domains of duckdns.org) to an IP of your choice. The service is completely free, and doesn't require reactivation because it uses a cron job to renew your IP address automatically.

Go to [https://www.duckdns.org/] and click on one of the various "Sign in with..." buttons. PSA: "use reddit" is discontinued, so anything but that is fine.

This is what the screen should look like:

![Duck DNS](/assets/dns0.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Under "domains" chose a sub domain for your server, and then click on "add domain".
If everything went well and the sub domain is available, you should see something like this:

![Duck DNS](/assets/dns1.png){:class="img-responsive" style="box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);"}

Click on "install" and then keep "linux cron" as the selected "Operating Systems". Choose your subdomain and follow the instructions carefully.
Once you get to the "what now?" section, you can either do what they say, or if you don't really mind, you can leave it and be just fine. 

## Summary
Congratulations! You have successfully set up a Live Apache Server with Oracle Cloud and a custom domain. You can now host your own website or web application on your own server. You can also use this guide as a reference for future projects involving Apache Servers, Oracle Cloud, and custom domains. Remember that right now, if you try to connect to your server through your domain, you will see a premade page from Apache. You can change that in any way or form you want by editing the files in /var/www/html or by using your own web application.

# Additional Resources
- [Oracle Cloud Documentation](https://docs.oracle.com/en-us/iaas/Content/home.htm): This website contains all the documentation you will ever need for Oracle Cloud. It's good to know but not fundamental for this guide.
- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/): Really comprehensive guide on how to use Apache. If you have any doubts on any configuration of the service, this is the place to go.
- [Duck DNS Documentation](https://www.duckdns.org/spec.jsp): One of the easiest yet most powerful documentation I've ever seen. It's really easy to understand and it's really well written.
- [BYU ITC 210](https://github.com/BYU-ITC-210/BYU-ITC-210.github.io): This is the repository for the ITC 210 class at BYU. It contains a lot of useful information, especially if you are interested in taking that class in the future. It's thank to this class that I learned how to use Jekyll, markdown, how to setup an apache2 server, and how to create a website from scratch, using HTML, CSS, JavaScript, and php.