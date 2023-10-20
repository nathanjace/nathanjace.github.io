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
![Oracle Sign Up](/assets/signup0.png){:class="img-responsive" style="border: 5px solid #555;"}

Fill out the form, make sure that you are a human and not a robot and then click on "Verify my email".
![Oracle Sign Up](/assets/signup1.png){:class="img-responsive"}

Continue with the sign up process by filling out the form and clicking on "Continue" at the end of the page.
Make sure to click on Individual under Customer Type, and then to pick a name that you will remember for your Cloud Account Name.
![Oracle Sign Up](/assets/signup2.png){:class="img-responsive"}

You will then be asked to insert your Address Information and to verify your phone number.
![Oracle Sign Up](/assets/signup3.png){:class="img-responsive"}

You then will have to "Add a payment verification method". It's important to notice that "You won't be charged unless you elect to upgrade the account". You won't be charged unless you want more out of your account, so don't worry about it.
![Oracle Sign Up](/assets/signup4.png){:class="img-responsive"}