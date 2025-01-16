# Instructions

The backup server in the `Stratos DC` contains several template XML files used by the Nautilus application. However, these template XML files must be populated with valid data before they can be used. One of the daily tasks of a system admin working in the xFusionCorp industries is to apply string and file manipulation commands!

Replace all occurances of the string `About` to `Marine` on the XML file `/root/nautilus.xml` located in the backup server.

# Solution

ssh into backup server: `ssh clint@172.16.238.16`

To check if the file exists, run this: `sudo bash -c 'if [ -f /root/nautilus.xml ]; then echo "File exists."; else echo "File does not exist."; fi'`

Or become the root: `sudo su -`

Check all the lines in the "/root/nautilus.xml" file that include the word "About”: `cat /root/nautilus.xml  |grep About`
