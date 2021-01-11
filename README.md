# Hisense-Amazon-Enabler: a patch to enable Amazon Prime Video app on Hisense Smart TVs
Have you ever wanted to watch Amazon Prime Video from your Hisense TV without connecting a PC? Now you can.

## Setup
First of all, you need access to the FTP interface of your television. It's quite easy to do using a UART to USB interface. I have written a small guide on xda-developers: [click here](https://forum.xda-developers.com/showpost.php?p=68737765&postcount=84)

Using a FTP client (I suggest [WinSCP](https://winscp.net/eng/download.php)) open the folder <code>/3rd_rw/appdata/launcher/data</code> and find an .xml file called <code>Category_xx_(bunch of numbers).xml</code>. In my case it was <code>Category_83_1596542795.xml</code>.<br><i>Note: if you by chance have more than one, delete all of them and restart the TV, which will generate a new (and hopefully only) one.</i>


This file contains the shortcuts to all the apps in the so called "Premium" category, which is the top one in the main menu (where you can open Netflix, Youtube and so on). Add the following piece of code to add a new shortcut to the Amazon Prime Video app:

<code></code>

Then, transfer these new files:
* <code>/3rd_rw/appdata/launcher/res/amazon.png</code>
* <code>/3rd_rw/internet_browser/bws_profile.ini</code>

Restart the TV and you can finally launch the Amazon Prime Video app.

## How does it work?
I found out that changing the Smart TV region to "UK" created a new Category xml file with a shortcut to the Amazon Prime Video app. Unfortunately, I couldn't keep the region as it breaks the DTV side. Moreover, Amazon Prime was UK only, meaning that all the shows were in English.

First of all I fixed the Amazon region. I plugged my UART to USB interface in the TV and captured the debug log when the app was started. In particular you see
<code></code>
The app configuration file <code>/3rd/internet_browser/apps/amazon_ruby/bws_profile.ini</code> contains the following data
<code></code>
As you can see, the app connects to the only non-commented URL, which is the UK one. One could remove the <code>#</code> to the EU one and enjoy the shows. However, this file lies in the read-only partition of the TV, and in order to edit one should create a modded firmware and hope not to break the TV.
However, in the log you can also see
<code></code>
The file <code>/3rd_rw/internet_browser/browser_config.ini</code> acts as an app configuration override. So I copied the content of <code>bws_profile.ini</code> in <code>browser_config.ini</code> and made the tweaks. It worked! The app reads this configuration override file and opens the correct URL. 

There's still one problem left: launching another app, like Youtube, still redirects to Amazon Prime Video. After some digging, I found out that another configuration override is <code>/3rd_rw/internet_browser/bws_profile.ini</code>. So I renamed <code>browser_config.ini</code> to <code>bws_profile.ini</code> and all worked nicely. Launching other apps no longer direct you to Amazon. 
