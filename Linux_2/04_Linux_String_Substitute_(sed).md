# Instructions

There is some data on `Nautilus App Server 3` in `Stratos DC`. Data needs to be altered in several of the files. On `Nautilus App Server 3`, alter the `/home/BSD.txt` file as per details given below:

a.  Delete all lines containing word `following` and save results in `/home/BSD_DELETE.txt` file. (Please be aware of case sensitivity)

b.  Replace all occurrence of word  `the` to `their` and save results in `/home/BSD_REPLACE.txt` file.

`Note:`  Let's say you are asked to replace word `to` with `from`. In that case, make sure not to alter any words containing the string itself; for example up**to**, contribu**to**r etc.

# Solution

ssh banner@stapp03

sudo su -

This command reads the contents of the file `/home/BSD.txt` and filters out the lines that contain the word "following", displaying them as output: `cat /home/BSD.txt |grep following`
