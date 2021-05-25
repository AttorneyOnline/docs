# Common Errors/Bugs (both content and generic)

It's regretful how I have to type this section up (considering #tech_support and #content exists on the AO Discord), but for the uninitiated who don't know how to use the Discord search bar, this document is made just for you.

## My client won't start!

Okay, there's 4 different issues you'll run into for this one, to which there's 3 solutions. I'll go through each issue one-by-one:

### I got an error pop-up saying either the app didn't start correctly (0xc000007b) or MSVCP140_1.dll is missing!

This one, thankfully for you, is pretty simple. The tl;dr here is that your computer's complaining about not having some files that didn't come with AO (as we would've hoped by now that your probably-massive Steam library beat you to the install first).

Just download and install [this](https://aka.ms/vs/16/release/vc_redist.x86.exe), and it'll be up and running!

### "It looks like your client isn't set up correctly"

If you get this error, probably means you did one of two things:

 - You downloaded only the executable and the DLLs needed for it
 - The download that your non-Vanilla server gave you is fucked up.

 The solution for both those scenarios? Download the latest *official* release of the client, or get someone from your server's Discord to give you a download that includes at *minimum* the `base\themes` folder. **Please note that you won't be able to join any servers as a character until you have downloaded and installed that servers' `characters` folder.**

### My client's .exe disappeared / My Antivirus thinks this program's a virus!

Tell your antivirus to shut up by adding an exception for AO's folder. Google's your best friend on how to do this, and in the case of "my client's .exe disappeared", either go to AO2 Github Repository and [download the latest version](https://github.com/AttorneyOnline/AO2-Client/releases/) of the client, or re-extract from the files that your server has provided you.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk3MTkzNzkzOSw4ODM3NDg4NDAsMTAyOD
UyMTc2OV19
-->