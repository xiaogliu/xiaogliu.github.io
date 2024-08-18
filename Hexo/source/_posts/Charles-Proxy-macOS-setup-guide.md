---
title: Charles Proxy macOS setup guide
date: 2024-08-18 14:49:09
tags: tools
---
## Context

Charles is an HTTP proxy / HTTP monitor / Reverse Proxy that enables a developer to view all of the HTTP and SSL / HTTPS traffic between their machine and the Internet. This includes requests, responses and the HTTP headers (which contain the cookies and caching information). Ever since I started using Charles my testing life has become much easier.

In this guide I will help you set up the Charles in macOS to view the iOS application’s traffic.

You can download a free trail from the [Charles website](https://www.charlesproxy.com/download/). Make sure to download the macOS dmg file.

## Installation

Double click on the dmg file. (In my case the version was charles-proxy-4.5.1.dmg)

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/01.png)

Click on the Agree button

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/02.png)

Move the Charles app to the Application Folder

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/03.png)

Open Charles from the Application folder

On your browser open ‘google.com’, probably you are going to see some error like this one below.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/04.png)

Check the network calls under the www.google.com

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/05.png)

This is because we have not configured the Charles Root Certificates in your machine.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/06.png)

Follow the images.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/07.png)

After installing the Charles Root Certificate, Open your Keychain Access. Search for Charles, now you may see something like this i.e. Certificate icon with (X) mark. It indicates that we have not trusted the certificate yet. We need to trust the certificate first.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/08.png)

Double click to open the Certificate. User Trust settings choose Always Trust.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/09.png)

Now you should see the certificate icon turns blue with a (+) mark on it. If that’s the case you have trusted the Certificate.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/10.png)

Now go to SSL Proxying Settings

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/11.png)

You need to add wild cards, so that you can get network calls from all the connected hosts. Add the following . and choose OK.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/12.png)

Now we need to install the Root Certificate on the Test Mobile Device. We need to make sure the Device under test and your Mac should be on the same network. Follow the screen.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/13.png)

You should see the IP address along with the port address. Please note this one.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/14.png)

Now go to the Wi-Fi settings, Tap on the info icon. and configure the proxy as follows. Please note this IP address and Port number will be used for configuring the Proxy.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/15.jpg)

Tap on the info icon

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/16.png)

Tap on the Configure Proxy

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/17.png)

Select the Manual Option

Now enter the IP and Port number shown in the Charles.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/18.jpg)

Now go to the Safari and enter the following url

chls.pro/ssl

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/19.png)

You should see a connection request at Charles now, Click on Allow.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/20.png)

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/21.jpg)

At the moment if you see the Safari browser, you should see something similar. Tap on Allow. Once the configuration profile is downloaded, you will get an indication. After that go to Settings > General > Profiles. User the Downloaded profile you should see the one we are looking for. Open the Profile.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/22.png)

Tap on the install.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/23.png)

You may need to enter the Phones unlock code to install the profile. Once the Profile is installed successfully, you should see the verified tick ✅ mark.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/24.png)

Go to the following location and enable the full trust for the root certificate. Follow the screenshot reference below.

`Settings > General > About > Certificate Settings > `

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/25.jpg)

That’s all you need to do. You may open your test application and see the network calls at Charles. For the demo purpose, I have used the Flipkart application and searched for an ‘iphone’

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/26.jpg)

You can see the network call and its contents.

![Installation](https://raw.githubusercontent.com/xiaogliu/xiaogliu.github.io.image/main/config_charles/27.png)
