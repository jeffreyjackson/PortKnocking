#PortKnocking
============

##Port Generation Algorithm
Uses a md5 hash of a file as key (Any file could be used as key)
Uses md5 hash of time for “randomness”
Server and client are synced by NTP
Hexadecimal segments are converted to decimal representations.  
New hashes are created every minute, therefore port configurations change every minute.
Time synchronization is vital!


###How the Server Works
There are 4 parts to the server
BashWrapper
PerlGen
KnockScript
knockd

###BashWrapper
BashWrapper is responsible for:
Delaying execution until a new minute.
Calling PerlGen every minute.
Restarting knockd every minute.
Delays must be added to make sure PerlGen does not execute before the new minute has actually begun.

###PerlGen
Creating MD5 of current Time.
Reading in MD5 of Key (could be dynamically created)
XOR’ing the two.
Converting hex segments to decimal port numbers.
Writing knockd.conf to current directory.

###knockScript
Called by knockd as specified in the generated knockd.conf.
Knockd passes the first port knocked for selection of actions defined, as well as the IP of the knocker.
knockScript executes actions based on the selection port.
All configuration is done in knockScript for simplicity of knockd.conf.

###How the Client Works
There are three parts to the Client

###PerlKnock
Knocker
knockScript – for actions.

###PerlKnock
PerlKnock is called to generate the port numbers, same code as PerlGen.
PerlKnock can take the Action Port and server address as an argument.
PerlKnock passes this information as well as the dynamic ports to Knocker

###Knocker
Takes action and server arguments from PerlKnock.
Prompts with menu of actions and request for server address if no arguments are passed.
Performs Knocking with .1s delay between each port.
.1s Delay is hopefully enough to avoid TCP Routing differences.

###knockScript Choices
We have interesting actions that can be performed and e-mailed, for example:
QuickCam Snapshot
X11 Snapshot
Who/Last Log
Nmap Quick/FullScan on scanme.ath.cx – Updatedable from Internet
Nmap of Source IP
ChkRootKit Results
Reboot/Shutdown
Open/Close SSH Port
Forward/Kill ssh tunnel of HTTP Proxy to adelie.spd.louisville.edu.
The possibilities are endless.

###Support
Our client package supports
MacOSX
Linux-i686
Linux-Amd64
Windows – Cygwin (in progress)

Our server package supports
Anything Knockd can run on.
Only tested on Linux.
