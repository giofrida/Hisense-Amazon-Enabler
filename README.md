# Hisense-Amazon-Enabler: a patch to enable Amazon Prime Video app on Hisense Smart TVs
Have you ever wanted to watch Amazon Prime Video from your Hisense TV without connecting a PC? Now you can.

## Setup
First of all, you need access to the FTP interface of your television. It's quite easy to do using a UART to USB interface. I have written a small guide on xda-developers: [click here](https://forum.xda-developers.com/showpost.php?p=68737765&postcount=84)

Using a FTP client (I suggest [WinSCP](https://winscp.net/eng/download.php)) open the folder <code>/3rd_rw/appdata/launcher/data</code> and find an .xml file called <code>Category_xx_(bunch of numbers).xml</code>. In my case it was <code>Category_83_1596542795.xml</code>.<br><i>Note: if you by chance have more than one, delete all of them and restart the TV, which will generate a new (and hopefully only) one.</i>


This file contains the shortcuts to all the apps in the so called "Premium" category, which is the top one in the main menu (where you can open Netflix, Youtube and so on). Add the following piece of code to add a new shortcut to the Amazon Prime Video app:

```xml 
   <ObjectInfo>
      <objectId>3</objectId>
      <objectType>37</objectType>
      <shortcutAppFlag>1</shortcutAppFlag>
      <startCmd>amazonruby</startCmd>
      <Owner>
        <ownerId>0</ownerId>
        <ownerName/>
        <objUrl/>
      </Owner>
      <VideoOwners/>
      <ObjectNames>
        <Item>
          <languageId>0</languageId>
          <name>Amazon Prime Video</name>
        </Item>
        <Item>
          <languageId>1</languageId>
          <name>Amazon Prime Video</name>
        </Item>
      </ObjectNames>
      <ObjectPictures>
        <Item>
          <languageId>1</languageId>
          <pictureUrl>/3rd_rw/appdata/launcher/res/amazon.png</pictureUrl>
          <pictureMd5>BF89AC8186B3E1F869741DF2DB2138E8</pictureMd5>
          <alienPicMark>0</alienPicMark>
          <recBackPicUrl/>
          <recBackPicMd5/>
          <alienBackPicMark/>
          <PicInfos/>
        </Item>
      </ObjectPictures>
      <objectUrl>amazonruby</objectUrl>
      <objectPackageName/>
      <ObjectDescs/>
      <OtherDescs/>
      <score/>
      <adCode/>
      <posNum>1</posNum>
      <tmplFlag>1</tmplFlag>
      <picRatio>115</picRatio>
      <XValue>0</XValue>
      <YValue>0</YValue>
      <Length>396</Length>
      <Width>396</Width>
      <reflectionFlag>0</reflectionFlag>
      <movableFlag>0</movableFlag>
      <Installer>1</Installer>
      <description/>
      <date/>
      <time/>
   </ObjectInfo>
```

I haven't tested this, but you should use a unique ID number in ```<objectId>3</objectId>```.

Then, transfer these new files:
* <code>/3rd_rw/appdata/launcher/res/amazon.png</code>
* <code>/3rd_rw/internet_browser/bws_profile.ini</code>

Restart the TV and you can finally launch the Amazon Prime Video app.

## How does it work?
I found out that changing the Smart TV region to "UK" created a new Category xml file with a shortcut to the Amazon Prime Video app. Unfortunately, I couldn't keep the region as it breaks the DTV side. Moreover, Amazon Prime was UK only, meaning that all the shows were in English.

First of all I fixed the Amazon region. I plugged my UART to USB interface in the TV and captured the debug log when the app was started. 
In particular you see

<pre><code>[amazonruby]>**************SET AMAZON RUBY TYPE************
[amazonruby]>/3rd/internet_browser/browser: unrecognized option '--http-cache-size=15'
[amazonruby]>/3rd/internet_browser/browser: unrecognized option '--remote-debugging-port=9222'
<b>[amazonruby]>Enter file name /3rd_rw/internet_browser/bws_profile.ini</b>
[amazonruby]><APP_PROF> Calling -------> prof_load_profile
[amazonruby]><APP_PROF> Current line is: [HOME_PAGE]
[amazonruby]><APP_PROF> This line is SECTION_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_section_name
[amazonruby]><APP_PROF> Current line is: #page=https://atv-ext.amazon.com/cdp/resources/app_host/index.html?deviceTypeID=A3GTP8TAF8V3YG
[amazonruby]><APP_PROF> This line is BLANKLINE_T!
[amazonruby]><APP_PROF> Current line is: #page=https://atv-ext.amazon.com/blast-app-hosting/html5/index.html?deviceTypeID=A3T3XXY42KZQNP
[amazonruby]><APP_PROF> This line is BLANKLINE_T!
[amazonruby]><APP_PROF> Current line is: page=https://atv-ext-eu.amazon.com/blast-app-hosting/html5/index.html?deviceTypeID=A2B5DGIWVDH8J3
[amazonruby]><APP_PROF> This line is KEY_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_key_info
[amazonruby]><APP_PROF> Current line is: 
[amazonruby]><APP_PROF> This line is BLANKLINE_T!
[amazonruby]><APP_PROF> Current line is: 
[amazonruby]><APP_PROF> This line is BLANKLINE_T!
[amazonruby]><APP_PROF> Current line is: [RESOLUTION]
[amazonruby]><APP_PROF> This line is SECTION_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_section_name
[amazonruby]><APP_PROF> Current line is: width=1280
[amazonruby]><APP_PROF> This line is KEY_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_key_info
[amazonruby]><APP_PROF> Current line is: height=720
[amazonruby]><APP_PROF> This line is KEY_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_key_info
[amazonruby]><APP_PROF> Current line is: 
[amazonruby]><APP_PROF> This line is BLANKLINE_T!
[amazonruby]><APP_PROF> Current line is: [MOUSE_MODE]
[amazonruby]><APP_PROF> This line is SECTION_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_section_name
[amazonruby]><APP_PROF> Current line is: enablemouse=0
[amazonruby]><APP_PROF> This line is KEY_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_key_info
[amazonruby]><APP_PROF> Current line is: 
[amazonruby]><APP_PROF> This line is BLANKLINE_T!
[amazonruby]><APP_PROF> Current line is: [OSD]
[amazonruby]><APP_PROF> This line is SECTION_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_section_name
[amazonruby]><APP_PROF> Current line is: use_2nd_osd=0
[amazonruby]><APP_PROF> This line is KEY_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_key_info
[amazonruby]><APP_PROF> Current line is: 
[amazonruby]><APP_PROF> This line is BLANKLINE_T!
[amazonruby]><APP_PROF> Current line is: [BACKGROUND]
[amazonruby]><APP_PROF> This line is SECTION_T!
[amazonruby]><APP_PROF> Calling -------> prof_get_section_name
[amazonruby]><APP_PROF> Current line is: backgmed=0
[amazonruby]><APP_PROF> This line is KEY_T!
</code></pre>

The app configuration file <code>/3rd/internet_browser/apps/amazon_ruby/bws_profile.ini</code> contains the following data
<pre><code>[HOME_PAGE]
#page=https://atv-ext.amazon.com/cdp/resources/app_host/index.html?deviceTypeID=A3GTP8TAF8V3YG
#page=https://atv-ext.amazon.com/blast-app-hosting/html5/index.html?deviceTypeID=A3T3XXY42KZQNP
page=https://atv-ext-eu.amazon.com/blast-app-hosting/html5/index.html?deviceTypeID=A2B5DGIWVDH8J3


[RESOLUTION]
width=1280
height=720

[MOUSE_MODE]
enablemouse=0

[OSD]
use_2nd_osd=0

[BACKGROUND]
backgmed=0
</code></pre>

As you can see, the app connects to the only non-commented URL, which is the UK one. One could remove the <code>#</code> to the EU one and enjoy the shows. However, this file lies in the read-only partition of the TV, and in order to edit one should create a modded firmware and hope not to break the TV.

Nevertheless, in the log you can also see

<pre><code>[amazonruby]>Don't set PATH
[amazonruby]>int main(int, char**) : config file path = /3rd_rw/internet_browser/browser_config.ini
[amazonruby]>int main(int, char**)
<b>[amazonruby]>Enter file name /3rd_rw/internet_browser/browser_config.ini</b>
[amazonruby]><APP_PROF> Calling -------> prof_load_profile
[amazonruby]>load profile fail,return
[amazonruby]>enable_log 0
[amazonruby]>enable_log 0
</code></pre>

The file <code>/3rd_rw/internet_browser/browser_config.ini</code> acts as an app configuration override. So I copied the content of <code>bws_profile.ini</code> in <code>browser_config.ini</code> and made the tweaks. It worked! The app reads this configuration override file and opens the correct URL. 

There's still one problem left: launching another app, like Youtube, still redirects to Amazon Prime Video. After some digging, I found out that another configuration override is <code>/3rd_rw/internet_browser/bws_profile.ini</code>. So I renamed <code>browser_config.ini</code> to <code>bws_profile.ini</code> and all worked nicely. Launching other apps no longer direct you to Amazon. 
