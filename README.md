# June_SECDSM_CTF
Write up for the 2022 June edition of the SecDSM mini-CTF event.

## Challenge
Given the following punch card, figure out what the flag is.
![r3tr0](https://user-images.githubusercontent.com/101227395/171697595-24b3c275-aeab-4c5c-a21f-910c9af463df.jpg)

Challenge created by _@ArchMafia_

## Walkthrough
### Step 1
I already know that we're working with a punch card, but is there any other information that I can glean from the provided card?  In fact, there was.  On the left-hand side, the punch card has some identifying information on where it was made:
![step 1](https://user-images.githubusercontent.com/101227395/171699951-93de0fe3-a316-48c6-a7dc-f229e0f8b30d.jpg)

Performing a search with the following key terms _masswerk punch card reader_ led me to the site https://www.masswerk.at/cardreader/.

### Step 2
Tried to upload the image provided for this challenge, but got an error message stating **"Sorry, this file doesn't look like a punch card created with the Virtual Keypunch."**  Hmmm, maybe I should try changing the picture format from '.png' to '.jpeg'?  No luck, the error persists.  Looks like this was going to be a little harder than I thought.

### Step 3
After doing some additional searching and getting nowhere with other online punch card readers, I decided to try my hand at figuring this out through trial and error.  Luckily, the previously mentioned cardreader site had a link to [The Virutal Keypunch](https://www.masswerk.at/keypunch/).  Now it was time to RTFM and figure out what the punch card was telling me.

### Step 4
After LOTS of trial and error, I was able to recreate the punch card and get the following output: C3F0B5E5CD487A099FE6869A906CEDC4
![download](https://user-images.githubusercontent.com/101227395/171728556-7f74e9ca-2d96-4295-a8ff-9959383be645.png)


### Step 5
Tried throwing the output into [CyberChef](https://gchq.github.io/CyberChef/), nothing tangible is returned.  Could it be something else?  Does this exist somewhere else on the internets?

### Step 6
Ran a search for 'C3F0B5E5CD487A099FE6869A906CEDC4' via Google, which gave me back an interesting result from the site md5hashing[.]net.  Well, that makes sense.  32 characters, looks like hex.  Ok, let's throw it in [Crackstation](https://crackstation.net/) and see if it's already been solved.  I wonder what it might say...
![image](https://user-images.githubusercontent.com/101227395/171735964-fcbdcfbc-20ff-4953-a74e-d32abb1b2a14.png)

![throwing-phone](https://user-images.githubusercontent.com/101227395/171748159-024d93c3-1351-48e7-b8c1-f9892eff376a.gif)


### Step 7... or Step 2 Part 2?
At this point, I took a break and happened to dilly dally in the SecDSM Discord channel (*cough* which you can join here: https://discord.com/invite/aqcDKzVYw3 *cough*).  Got a tip off that the image may not be all it's cracked up to be.  Someone is running `atrings` against the image?  Well why would you do that...

Using the fantastic site https://aperisolve.fr/, I uploaded the punch card image to see what kind of shenanigans that _@ArchMafia_ might be up to.  Enabling all of the options other than *'I've got a password!'*, I saw something reallllllyyyy interesting at the bottom of the strings output: ***prettycolors.png***.  Is this a **STEGO CHALLENGE?!**  Dope!

### Step 8
Thanks to aperisolve doing all of the heavy lifting for me, I was able to extract another image:
![prettycolors](https://user-images.githubusercontent.com/101227395/171733832-0891aece-99b6-4a86-a27c-8c8378cfeceb.png)

Well, the file name is right, it does have some pretty colors...  But what do I do with- \*gestures wildly\*- *this*?  

### Step 9
I try running prettycolors.png through aperisolve as well.  Nothing of value is returned.  I felt like I was getting nowhere with this new picture, so I went back and checked a few things from the original image.  

2 items stuck out to me upon further review:
1) The strings contained a weird block of code: 
`+++++ +++[- >++++ ++++< ]>+++ +++++ ++++. <++++ +[->+ ++++< ]>.<+ +++++
++[-> ----- ---<] >---- -.<++ +++[- >++++ +<]>+ +++++ ++++. <++++ ++[->
+++++ +<]>+ .+.-- -..<+ ++[-> +++<] >+++. <+++[ ->--- <]>-- --.<+ +++++
++[-> ----- ---<] >---- -.<++ ++++[ ->+++ +++<] >++++ +.<++ ++++[ ->+++
+++<] >+.<+ ++[-> ---<] >-.+. --.++ +++.+ .---. .<+++ [->++ +<]>+ ++.<+
+++[- >---- <]>-. +.<++ +[->+ ++<]> +.--- ----. <++++ ++[-> ----- -<]>-
----- -.<++ +++[- >---- -<]>- .<+++ +[->- ---<] >---. ---.< +++++ +++++
[->++ +++++ +++<] >++++ +.<++ +[->- --<]> ----. .++.< ++++[ ->+++ +<]>+
.<+++ [->-- -<]>- --.<+ ++[-> +++<] >++++ +.<++ ++[-> ----< ]>.<+ ++[->
+++<] >++.< +++++ +++[- >---- ----< ]>--- ----- -.<++ +++++ +[->+ +++++
++<]> +++++ +++++ +.<++ +[->- --<]> --.++ .++++ .<+++ +++++ [->-- -----
-<]>- ----- .<+++ ++++[ ->+++ ++++< ]>+++ +.<++ +++++ [->-- ----- <]>--
--.<+ +++++ +[->+ +++++ +<]>+ +++++ +++++ .---- -.<++ +[->+ ++<]> ++++.
----- --.<+ +++++ +[->- ----- -<]>- ----- ----- -.<++ +++++ +[->+ +++++
++<]> ++.++ ++.<+ +++++ ++[-> ----- ---<] >---- --.<+ +++++ +[->+ +++++
+<]>+ +++++ +++++ ++.<+ ++[-> +++<] >++.< +++++ +++[- >---- ----< ]>---
----- -.<++ +++++ +[->+ +++++ ++<]> .<+++ [->++ +<]>+ ++.<+ +++++ ++[->
----- ---<] >---- ----- ---.< +++++ +++[- >++++ ++++< ]>+++ ++.<+ ++[->
---<] >---. --.<+ ++[-> +++<] >+++. <+++[ ->+++ <]>++ ++.<`
This looks familiar and I could swear it's been used in other SecDSM CTFs.  Looking up some esoteric programming languages makes me believe this is Brainf***.  (this is a child-friendly write up)
2) Another file was found by the steghide option called 'getphreaky.txt'.  This text file contained the following: "9EEADi^^HHH]>65:27:C6]4@>^7:=6^;hCBce_a8_bI82B^42AE2:?4CF?49]H2G^7:=6"

These are definitely something.  I now have a pretty colors picture, some weird looking code block, and a garbled string.  Fun!

### Step 10
Running the Brainf*** through one of the *many* online interpreters- in this case, I used https://www.dcode.fr - gives me the following output: `Le Chiffre Indechiffrable: 
sffhym{kv-xmos-b-idqj-os-kv-my-rfdp}`.  I'll save that for now, cause we'll definitely need to decode that later, as it most assuredly looks like cipher text.  With the French in there, I'm guessing Vignere.  

### Step 11
Running the text from getphreaky.txt through [dcode.fr's cipher identifier](https://www.dcode.fr/cipher-identifier) gives me a clue that it is a ROT-47 cipher.  Classic.  This turns out to be an encoded URL - https://www.mediafire.com/file/j9rq4602g03xgaq/captaincrunch.wav/file, which as you might guess, leads me to a audio file with some rapid dial tones.  

### Step 12
The captiancrunch.wav has a lot of dial tones go by EXTREMELY rapidly.  I used http://dialabc.com/sound/detect/index.html to break it down into its associated values, resulting in `3#66#8#77#8#4#3#2#22#66#88#33#6#4#3#3#55#3#2#9#44#2#8#22#66#8#55#3#44#2#3` or `3 66 8 77 8 4 3 2 22 66 88 33 6 4 3 3 55 3 2 9 44 2 8 22 66 8 55 3 44 2 3` without the pound signs/hashes.  Using dcode.fr again, the string of numbers is turned into..... `DNTQTGDABNUEMGDDKDAWHATBNTKDHAD`.

![image](https://user-images.githubusercontent.com/101227395/171784657-13568455-142e-4df6-bc16-84b51c23a5d7.png)

Anyways, taking another look at the .wav file in Audacity, it looks like there may be some items I'm missing.  I try another site to translate the Dual-Tone Multi-Frequency (DTMF) signal, specifically https://unframework.github.io/dtmf-detect/#/.  From that, I get something similar, but just different enough:  `333#666#88#777#8#44#33#2#22#666#888#33#6#444#3#3#555#33#222#9#44#2#8#222#666#88#555#3#444#22#33`.  In order to capture this new string of numbers, I had to inspect the page, look at the specific field that was being filled in by numbers, then copy those numbers before they cleared away after 5 seconds.  :/  

Well I got it.  Okay, what does dcode give me now?

*FOURTHEABOVEMIDDLECWHATCOULDIBE*

Progress!

### Step 13
After thinking back on some of what the CTF master said about this challenge, _@ArchMafia_ stated that their inspiration came from some CTF Write Ups for other events.  I haven't touched the prettycolors.png in a while, so I tried doing a reverse image search using Google and using the search teram 'CTF Write Up'.  The very first image result that came back looked SUPER SIMILIAR to the image we had pulled out of the punch card.  Once we've clicked on that first image, lo and behold we have a name for this color scheme: **Hexahue**.
![image](https://user-images.githubusercontent.com/101227395/171888497-ae465709-db77-4c5e-8136-0d87f89608d8.png)

Using https://www.boxentriq.com/code-breaking/hexahue, I decode the pretty colors and got the message "A SEGMENT OF 8 IS UNDOUBTEDLY GREAT.  BUT A SEGMENT OF 7 WILL OPEN THE GATE..."   Sounds ominous...  And maybe there is something else out there in some other CTF Write Ups about the numbers in Step 12.

### Step 14

[TBD]
