# Bandit
Bandit exercise walkthrough

#level1
opening a - named file 
cat < - or cat ./-

#level2
spaces in the file name
- use tab complete 
- use \ to escap the space between the names 
- use single quotes '<file name>'

#level3
hidden file
- use ls -la to list the hidden file
- cat .<filename> to open it

#level4 
opening a bunch of file for a password/strings
- cat * 
- file * to determine the file type (odd man out)
- strings *
 
#level5
searching in a bunch of directories for a file with 1033 bits size, non executable and readable
- find /<dir> \! -executable -size 1033c -exec file {} \;

#level6
searching in a buch of directories and files for a file owned by user bandit7, group bandit6 and 33bytes in size
- find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
(or)
- find / -user bandit7 -group bandit6 -size 33c 2>/dev/null | while read file; do cat $file; done

#level7 
serach for a work next to a word
- grep '<word>' <file>

#level8
search for the unique word from the list of many repeated words  
- sort <file> | uniq -u 
- here sort will order the list with the duplicates inline and uniq -u will grep the non-repeated word

#level8
search for a words preceeds by a bunch of '=' signs 
- strings -n 32 <file>

#level9
search for a words from a list preceeds by a bunch of '=' signs
- strings <file> | grep =====

#level10
to get the base64 decoded data
- base64 -d <file>

#level11
to get the word roated by lower and upper case of 13 characters 
- cat <file> | tr 'A-Za-z' 'N-ZA-Mn-za-m'
- tr is for translate, and we provided the range to rotate (for A-Z replace with N-Z and A-M, same for the lower case)

#level12
retrive the text from a hex dump data of a multi time compressed file 
- xxd -r <hexdump file> > <compressed file>
- file <compressed file> --> provides the compressed file format (say gzip)
- mv <compressed file> <compressed file>.gz 
- gunzip <compressed file>.gz
- file <one time uncompressed file> --> provides the compressed file format (say bzip2)
- mv <one time uncompressed file> <one time uncompressed file>.bz2
- bzip2 -d <one time uncompressed file>.bz2
- file <2nd time uncompressed file> --> provides the compressed file format (say gzip)
- mv <2nd tim uncompressed file>.gz
- gunzip <2nd time uncompressed file>.gz
- file <3rd time uncompressed file> --> tar archive 
- tar -xf <3rd time uncompressed file>
- file <4th time uncompressed file> --> tar 
- tar -xf <4th time file>
- file <4th time file> --> bzip2
- bzip2 -d <4th time file>
- file <5th time> --> tar
- tar -xf <5th time>
- file <6th time> --> gzip
- mv <6th time> <6th time>.gz
- gunzip <6th time>.gz 
- file <7th time> --> ASCII
- cat <7th time>

#level13
use ssh private key to login
- ssh -i <private key> <username>@<machine IP>

#level14
connect to the localhost port 30000 and submit the current password 
- nc localhost 30000
- paste the current user password
(or)
- echo '<password>' | nc localhost 30000

#level15
connect to the localhost port 30001 and submit the current user password over openssl 
- openssl s_client -connect localhost:300001
- paste the current user password 

#level16
find the port b/w 31000 and 321000 and submit the current user password to a ssl port from the list
- nmap -Pn -p 31000-32000 -sV -min-parallelism 1000 localhost 
- openssl s_client -connect localhost:31790
- paste the current user password 
- copy paste the private key for the next user connect

#level17
find the change of word in the second file compared with the first
cat <file>* | sort | uniq -u | while read pass; do grep $pass <file>2; done
diff file1 file2

#level18
blocked with ssh without a shell or rshell, get a persistant shell while login with ssh 
ssh -p <port> user@machineIP -T /bin/sh  

#level19
SUID binary executed command as another user by taking input as argument 
- ./<binary> /bin/bash -p 
- to get the bash terminal of the executing user with the presistant privilege of the effective executing user 

#level20
execute a binary and connect with the localhost on the same machine with any listening port, pass the current user password and get the next user password 
- nc nvlp 4444 (section1)
- ./<binary> 4444 (section2)
- paste the current user password in the section1 and get the next user password 
(or)
- ./<binary> 4444 (section1)
- echo 'current user password' | nc -nvlp 4444 (section2) 
- get the next user password 
