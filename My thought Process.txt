1) Bandit0 was a pretty straightforward usage of the cat command so there you have it!

2) Bandit1, again another very basic use of the cat command.

3) Bandit2, since there were literal spaces in the file it was necessary for it to be in quotes:), or you could use forward slashes in front of each word....

4) Bandit3, I changed my directory and used the cat command again which I believe was pretty obvious to do!

5) Bandit4, Here as I read through the clue I knew I had to some how filter out the ASCII containing file from the lot. So I bumped into the file command and finished the job. The * here basically represents that the command has to go through ALL the files in the directory.

6) Bandit5, I knew I had to filter out a file here again, so I used the find command from the man page again and executed the cat command yet another time!

7) Bandit6, as usual I used the find command which got me into a kind of pickle here, it showed all the error messaged too. So I had to somehow filter again, thats when I bumped into 2>/dev/null which redirected all the error messages from stdout to /dev/null:)!

8) Bandit7, was a really basic use of the grep command...

9) Bandit8, yet another filter needed here, so I needed to segregate it in a way such that the least occuring passcode ie: which occurs only once, is sieved out. So I used SORT!

10) Bandit9, yeah I used cat and was taken off-guard there a lil bit but I used the strings command and solved it...!

11) Bandit10, pretty straight forward base-64 decryption, got that in a jiffy!! base64 -d <filename> just decodes it!

12)Bandit11, now here we've got a ROT13 encryption to deal with. It's basically when each alphabet is equated to an alphabet 13 alphabets away (I hope that makes sense!). Consider it like a circle of alphabets. Now to decrypt we have to use the translate command (tr), now z is 13 alphabets away from n and a is 13 before m, but since its a loop it doesn't matter if its ahead or behind. We use a simple command to solve this. '<' is used just to make the file go through whatever command is on its left.

13)Bandit12, okay I admit it, here's where the real stuff start! Now while concatenating the text file, we can see some peculiar set of bytes in the beginning of the file which basically are called "MAGIC NUMBERS", how cool, isnt it? They uniquely identify the filetype, so that the OS doesnt need to look at the extensions to identify the format of the file. Here we need to know the magic numbers of Gzipped files= '1F 8B' and of Bzip2'd files= '42 5A 68'. Now since its a hexdump of a file im gonna revert it to its original binary form for easier accessing and transmitting to a different file. Now concatenating the reverted file we can see 1f8b! So rename the reverted file to a .gz extension and decompress it using gzip a pretty straightforward command. The final file will obviously lose this extension. So cat it again and we see 425a!! How fun! So now rename the file again to the .bz2 extension. Decompress it again. We can xxd the file to view the magic numbers, this shows that its again gzipped so, yeah do the needful and decompress. After this use cat or xxd to view internally and we see  data5.bin, a file!! So it has to be an archive, lets use "tar" an archiving and unarchiving utility, so extract files from the archive using a simple command, and we will end up with another archive which is data5.bin. Then use tar to -xf from data5.bin, we get data6.bin, again -xf files to get data8.bin, viewing the contents of data8.bin, we see its a gzipped file, so unzip it and we get a file called data8. Cat data8, and voila, there is the password.

14) Bandit13,Here we get a private key to directly jump into the next level, but without knowing how to use it we're going nowhere. Here we have to exit the current level to use the private key and login to the next level. 1st we use the scp command (secure file copy ) because it uses the same authentication and security as in the login sessions. Now after this we just login with the private key, -i is used to select files from which the private key for public key authentication is read. Yeah we're in bandit 14.

15) Bandit14, Here we use netcat, it basically creates a TCP connection to the inputted port on the host. The current level's password location was given in the pevious level so while connecting to port 30000 input the current levels password to get the next one! :)

16) Bandit15, now this one uses ssl encryption, the secure sockets layer encryption, now OpenSSL is used to encrypt communication over networks that use Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols. Since we have the right key we can easily decrypt with a single line of command, just input current level password to get the next level's.

16) Bandit16, here we have to scan through some ports so we use a popular network scanning tool called nmap, here the -sV flag does a scan on the type of service and its version. We could have used -A for scanning every bit of detail but thats gonna take some time. Here after scanning nmap tells us that 5 ports are open, and from that only 2 ports use ssl, and from that one of them uses an echo service and other one uses an unknown service, kinda sus right? So I connect to that port using OpenSSL client and inputted the current level password and it gave me a private key to login to the next level. I exited the level and opened a common text editor called nano, and pasted the private key in there. To make it executable i changed mode using chmod and gave read, write, execute permissions to the owner of the file only, which is me! Then I logged in using ssh and the pvt key and, voila! Straight into level 17!

17) Bandit17, now here only 1 line between these 2 files are different and for that we use the diff command. Use diff password.old passwords.new, the 2nd line gives you the password to the next level.

18) Bandit18, the .bashrc file runs every time a terminal is opened or during logging in ssh, so this could mean that during ssh login we can remotely execute commands while logging in. So just type in the command you need to execute just next to the standard login command.

19) Bandit19, here this file is a little peculiar, the owner has got rws permissions. The s stands for set userID permission, and whoever plans on executing the file, they're gonna execute as the owner of the file and here if you look closely the owner of the file is Bandit20. So the moment you execute the file at that particular instant you're in Bandit 20, so we know the location of the password so while executing the file just type in the command needed for accessing the password as if you were in bandit20 and voila! There you have your password!

20) Bandit20, yeah, this is one of the most favorite levels of mine. Using netcat, we can create a connection server mode that listens for inbound connection to a port. So I connected to a random port, say 2222 using the flags -l, which is used for listening at that port to see if someone else has connected to it, -v which is the verbose flag that just provides a more detailed output about what the command is doing, and the -p flag for inputting the port to listen to. So suconnect apparently is a program that is used to connect to a given port using TCP, so i'm gonna run the program and input a port next to it. At that very instant the listener identifies that a connection was received and we have to input in the current level's password. So it checks if the passwords match and when it is matched it returns the next password. How convenient!

21) Bandit21, Hmmmmm, here we change directory to the given location and list the stuff present inside of it. We see a lot of cronjob files and instinctively I wanted to see what was inside the cronjob_bandit22 file because that is our next level. When concatenating it we see something peculiar, an address to a BASH script! So I catted that address to see what's inside of the script and I see that the password was concatenated to a different location. Its mode was also changed to Read and write for the owner and, only read for everyone else. So I simply used the cat command again and ended up with the password to the next level.












#If you read through all of this, I THANK YOU!
