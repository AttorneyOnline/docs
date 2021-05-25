# Basic Troubleshooting

It's regretful how we have to type this section up (considering #tech_support and #content exists on the [AO Discord](https://discord.gg/wWvQ3pw)), but for the uninitiated who don't know how to use the Discord search bar, this document is made just for you. ***Please note that unless the section states otherwise, all of these situations will occur ONLY on the Client Version of Attorney Online.***

## My client won't start!

Okay, there's 4 different issues you'll run into for this one, to which there's 3 solutions. I'll go through each issue one-by-one:

### I got an error pop-up saying either the app didn't start correctly (0xc000007b) or MSVCP140_1.dll is missing!

This one, thankfully for you, is pretty simple. The tl;dr here is that your computer's complaining about not having some files that didn't come with AO (as we would've hoped by now that your probably-massive Steam/GoG/Epic library beat us to the install first).

Just download and install [this](https://aka.ms/vs/16/release/vc_redist.x86.exe), and it'll be up and running!

### "It looks like your client isn't set up correctly"

If you get this error, probably means you did one of two things:

 - You downloaded only the executable and the DLLs needed for it
 - The download that your non-Vanilla server gave you is fucked up.

 The solution for both those scenarios? Download the latest *official* release of the client, or get someone from your server's Discord to give you a download that includes at *minimum* the `base\themes` folder. **Please note that you won't be able to join any servers as a character until you have downloaded and installed that servers' `characters` folder.**

### My client's .exe disappeared / My Antivirus thinks this program's a virus!

Tell your antivirus to shut up by adding an exception for AO's folder. Google's your best friend on how to do this, and in the case of "my client's .exe disappeared", either go to AO2 Github Repository and [download the latest version](https://github.com/AttorneyOnline/AO2-Client/releases/) of the client, or re-extract from the files that your server has provided you.

## My client crashed/froze while selecting a character and/or changing the selected emote!

Another case of "shut up, Mr. Antivirus". It's caused by your antivirus not liking the fact that AO's writing the `_on.png` buttons for an emote, and what some antiviruses just outright don't tell you what they're doing, which results in the crash and/or freeze. Add an exception to AO's folder, and your problem will be fixed.

Here's what Avast would look like if this issue occurs (and you don't have it set to silent mode):
![](https://puu.sh/HJxe0/aad0af8617.png)

## I can't connect to the Masterserver!
***This is for both Client and WebAO users***
Sorry to our Linux and MacOS buddies, but this solution's built for Windows (until a Linux chad like Longbyte walks in and adds something for you buckos. MacOS, we're sorry, but you're underappreciated because Apple is a nightmare to work with. Use bootcamp and get a Windows install going).

This is less of a "how to fix" and more of a "let's get some basic troubleshooting as to what's going on here". If neither comparison with the screenshots provided give you an idea of what to do, go to the [Official AO Discord](https://discord.gg/wWvQ3pw) with your own version of the screenshots prepared, as chances are you *will* be asked to provide them.

First, let's check to see if your computer can actually *see* the masterserver's domain.

 - Open the Command Prompt
 - Type `nslookup master.aceattorneyonline.com` and hit Enter.
 If you don't get a response along the lines of the below image from the Command Prompt, join the AO Discord and hit `#tech_support` for more help. If you do see something like this, then move on to the next step.
 ![](https://cdn.discordapp.com/attachments/278576491191599104/825387567138471986/unknown.png)
 - Open [this link](http://52.47.209.216:27016/). 
This time, if you don't see something like the below image, then join the AO Discord and hit `#tech_support`
![](https://puu.sh/HJxfl/42cc1034d2.png)

---
Thanks to the Official AO Discord and theDev Team for the solutions to these solutions.
Screenshots are provided by Crystal#5638
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkwNDA0NDUyMl19
-->