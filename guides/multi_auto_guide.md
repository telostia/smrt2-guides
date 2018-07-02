----------------------------------------------------
SMARTSPACE COIN SIMPLE AUTO MASTER NODE SETUP GUIDE 
----------------------------------------------------

-----------------------------
Windows wallet setup (4 MN in one vps)
-----------------------------

Download windows wallet from these locations:
for 32bit: 
https://github.com/smrt-crypto/smrt/releases/download/v1.1.0.5/smrt-qt-win32.exe
for 64bit: 
https://github.com/smrt-crypto/smrt/releases/download/v1.1.0.5/smrt-qt-win64.exe

run it. if you cannot get it to sync. got to windows start and type:
%appdata% 
you should see "roaming" click on it. it will take you to a folder.

find the "SMRT" folder and double click on it.
right click on "smrt.conf" and open it with notepad or any text editor.
When file is loaded, add more nodes by copy and paste this:

* addnode=45.77.52.239:52310
* addnode=31.171.251.72:52310
* addnode=167.99.70.168:52310
* addnode=144.202.78.25:52310
* addnode=149.28.37.210:52310
* addnode=74.108.58.91:52310
* addnode=206.189.186.158:52310
* addnode=107.174.250.215:52310

save file and exit.

Now to get a genkey for the linux side.

Open the Smrt Coin Desktop Wallet. 
Go to Tools -> "Debug console - Console" 
Type the following command: masternode genkey

Do this task 4 times and copy and past 4 genkeys onto a text file.

For e.g I will be using notepad. You can use any text editor for this as it is just to help you organize the genekeys.

Copy the first code by double click on the codes and right click copy.

Now move on to linux vps setup.

-----------------------
### Linux VPS setup
----------------------
Log into your linux Vps and type each commands and press enter on each lines below:

* cd
* wget https://raw.githubusercontent.com/telostia/smrt2-guides/master/guides/smart_auto.sh
* bash smart_auto.sh 

when asked to paste genekey. right click to paste your genkey in.(This is the first genkey you copied earlier)

It will now do its work and automatically "watches" using a command getinfo which updates every 2 seconds. You can see the blockcount moving.
Now if you open up the explorer at: http://explorer.smartspace.ml:3001 look the the blockcount on the bottom right corner of the page. Pay attention to the block count.
For e.g. if blockcoin is 70000. Now focus on the vps and see if it reaches 70k. Once it reaches close to the 70000 blockcount earlier it is now time to move on to the
next step.

Normally with a single masternode setup we move onto the windows wallet, however since we are doing multiple masternode setup, close the getinfo watch screen by holding
the "control" and press "c". This will close itself to the standard command line. 
We now have to copy the data files directory onto a another folder named after the MN count.
the command to copy is as follows:

(This will duplicate the data from first data directory onto 2nd data directory)
* cp -r -p /root/.smrt /root/.smrt2
(This will duplicate the data from first data directory onto 3rd data directory)
* cp -r -p /root/.smrt /root/.smrt3
(This will duplicate the data from first data directory onto 4th data directory)
* cp -r -p /root/.smrt /root/.smrt4

Now delete the 2nd,3rd and 4th peers.dat and wallet.dat as its recommended to have these files uninque.
* rm /root/.smrt2/peers.dat
* rm /root/.smrt2/wallet.dat

* rm /root/.smrt3/peers.dat
* rm /root/.smrt3/wallet.dat

* rm /root/.smrt4/peers.dat
* rm /root/.smrt4/wallet.dat

Editing the config files to make it unique.

We will first start with the config file for the 2nd masternode.
Do the following command:
* nano /root/.smrt2/smrt.conf

while inside the file, change the following details according to your IP.


Edit only the following: 
* rpcport=(increment the number by 1, so 52311 becomes 52312 goes here)
externalip=(your 2nd ip goes here)
bind=(your 2nd ip and port goes here)
masternodeprivkey=(your 2nd masternode genkey goes here)

sample config additions: (note that all details here are just dummy values, fill your own in)
* rpcport=52312
* externalip=144.0.1.2
* bind=144.0.1.1:52310
* masternodeprivkey=891123456hYo4j755555LTW2bNw4TkPdjibpnDhZ000000Ri99Q
 
start the 2nd masternode with the following command:
* smrtd -datadir=/root/.smrt2

you can use the usual smrt-cli commands with the following:

(for getinfo)
* smrtd -datadir=/root/.smrt2 getinfo
(for master status checks)
* smrtd -datadir=/root/.smrt2 masternode status


The process for the 3rd and 4th masternode config is the same as the 2nd masternode so it will look like this:

start the 2nd masternode with the following command:
* smrtd -datadir=/root/.smrt3

(for getinfo)
* smrtd -datadir=/root/.smrt3 getinfo
(for master status checks)
* smrtd -datadir=/root/.smrt3 masternode status

Note that I only needed to change the .smrt folder from .smrt2 to .smrt3. 4th masternode would be be changed to .smrt4 and so on.


And now to complete the windows wallet+linux masternode pair.


---------------------------------
Windows wallet to add masternode for 2nd,3rd and 4th masternode
---------------------------------

1.   Open the SMRT Coin Desktop Wallet. 
2.   Go to RECEIVE and create a New Address: MN1, MN2, MN3 and MN4
3.   Send 5000 smrt to MN1, MN2, MN3 and MN4
4.   Wait for confirmations. 
5.   Go to Tools -> "Debug console - Console" 
6.   Type the following command: masternode outputs 
7.   Go to ** Tools -> "Open Masternode Configuration File" 


8.   Add the following entry: 
Alias Address Privkey TxHash Output_index 
?  Alias: MN1 
?  Address: VPS_IP:PORT 
?  Privkey: Masternode Private Key (paste the genkey you made earlier)
?  TxHash: First value from Step 6 
?  Output index: Second value from Step 6 
9.   Save and close the file. 
10.   Go to Masternode Tab. If your tab is not shown, please enable it 
from: Settings - Options - Wallet - Show Masternodes Tab 
11.   Click Update status to see your node. If it is not shown, close the wallet and 
start it again. Make sure the wallet is unlocked. 
12.   Open Debug Console and type: 
masternode start-alias 0 [alias] 

sample for mn1:
masternode start-alias 0 mn1

do steps 8 to 12 again but changing to 2nd txhash,genkey stored earlier and alias.
add another line onto masternode.conf like this:

* mn1 VPS_IP1:PORT GENKEY1 TXHASH1 OUTPUTS_NDEX1
* mn2 VPS_IP2:PORT GENKEY2 TXHASH2 OUTPUTS_INDEX2
* mn3 VPS_IP3:PORT GENKEY3 TXHASH3 OUTPUTS_INDEX3
* mn4 VPS_IP4:PORT GENKEY4 TXHASH4 OUTPUTS_INDEX4

sample masternode.conf:(note that port number remains the same for all masternodes)

* mn1 144.0.1.1:52310 13720437070f0df70dsf70dsf7007f1 f1d0f70fd707f07ds07f0s70fsd077f07dsa07f 1
* mn2 144.0.1.2:52310 23720437070f0df70dsf70dsf7007f2 f2d0f70fd707f07ds07f0s70fsd077f07dsa07f 0
* mn3 144.0.1.3:52310 33720437070f0df70dsf70dsf7007f3 f3d0f70fd707f07ds07f0s70fsd077f07dsa07f 1
* mn4 144.0.1.4:52310 43720437070f0df70dsf70dsf7007f4 f4d0f70fd707f07ds07f0s70fsd077f07dsa07f 0


Complete! Thank you for joining the revival smart coin - smart cloud. For typos, further enquiries or any other enquiries please see the discord channel.

 