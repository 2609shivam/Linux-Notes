# Linux Tips & Tricks

## Accessing "-" Filename
To view a file named "-", use:
```sh
cat ./-
```
## Accessing Files with Spaces in Their Names

To read a file with spaces in its name, use:

```sh
cat "file name with spaces" 
```
OR
```sh
cat file\ name\ with\ spaces
```
## Accessing Hidden Files in Linux

Hidden files in Linux start with a `.` (dot). To list them, use:

```sh
ls -a
```
To access a specific hidden file, use:
```sh
cat .hiddenfilename
```
## Finding and Accessing Human-Readable Files in Linux

The goal was to find the only human-readable file in the `inhere` directory.

### **Steps to Solve:**
1. **List all files, including hidden ones:**  
   ```sh
   ls -a
   ```
2. **Identify human-readable files:**
   ```sh
   file ./*
   ```
3. **Read the contents of `-file07'**
   ```sh
   cat ./-file07
   ```
## Finding a Specific File Based on Criteria

The goal was to find a file under the `inhere` directory with:  
✔ Human-readable text  
✔ 1033 bytes in size  
✔ Not executable  

### **Steps to Solve:**
1. **Find the file matching the given properties**
   ```sh
   find -type f -size 1033c ! -executable
   ```
2. **Read the file to get the password**
   ```sh
   cat ./maybehere-07/.file2
   ```
## Finding a File Based on User, Group, and Size

The goal was to find a file somewhere on the server that meets the following conditions:  
✔ Owned by **user** `bandit7`  
✔ Owned by **group** `bandit6`  
✔ Exactly **33 bytes** in size  

**Use the `find` command to locate the file:**
   ```sh
   find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
   ```
## Finding a word next to a specific word  in a .txt file

**Use the `grep` command to locate the word next to `millionth`:**
```sh
grep millionth data.txt
```
## Finding the only non repeated line in a .txt file

### **Steps to Solve:**
1. **Sort the file so duplicate lines are grouped together**
   ```sh
   sort data.txt
   ```
2. **Use `uniq -u` to find unique lines**
   ```sh
   sort data.txt | uniq -u
   ```
## Extracting Human-Readable strings from a File in Linux
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
### **Steps to Solve:**
1. **Identify the file type**
   ```sh
   file data.txt
   ```
2. **Extract human-readable strings**
   ```sh
   strings data.txt
   ```
3. **Filter only the strings with `=` characters**
   ```sh
   strings data.txt | grep '====='
   ```
## Decoding Base64-Encoded Files in Linux
The password is stored in a data.txt file.
```sh
base64 -d data.txt
```
## Decoding ROT13 Cipher in Linux
The password is stored in a data.txt file.
```sh
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
- `tr` stands for translate
- First half `A-M` becomes `N-Z`
- Second half `N-Z` becomes `A-M`
## Reconstructing and Decompressing a Hexdumped File
The password is stored in a data.txt file.
### **Steps to Solve:**
1. **Create a safe workspace in `/tmp`:**
   ```sh
   mkdir $(mktemp -d bandit12XXXX)
   ```
2. **Copy and move the data file:**
   ```sh
   cp ~/data.txt .
   mv data.txt data.hexdump
   ```
3. **Use `xxd` to reverse the hexdump into binary:**
   ```sh
   xxd -r data.hexdump data.bin
   ```
4. **Inspect the file type:**
   ```sh
   file data.bin
   ```
5. **Unpack each layer accordingly:**
   ```sh
   mv data.bin data.gz
   gunzip data.gz

   file data  # Example: might now say it's a bzip2 file
   mv data data.bz2
   bunzip2 data.bz2

   file data  # Maybe a gzip again?
   mv data data.gz
   gunzip data.gz

   file data  # Maybe a tar archive?
   mv data data.tar
   tar -xf data.tar

   # Keep checking 'file' and unpacking until you get a regular ASCII text file
   ```
## SSH Key Authentication and Accessing Protected System Files
The password is stored `/etc/bandit_pass/bandit14` and can only be read by user `bandit14`.
### **Steps to Solve:**
1. **Copy the private key to a writable temporary directory:**
   ```sh
   mkdir /tmp/mykeydir
   cp sshkey.private /tmp/mykeydir/
   ```
2. **Set proper permissions on the key:**
   ```sh
   chmod 600 /tmp/mykeydir/sshkey.private
   ```
3. **SSH into `bandit14` using the private key:**
   ```sh
   ssh -i /tmp/mykeydir/ssh.private bandit14@localhost -p 2220
   ```
   This tells SSh:
   - `-i sshkey.private`: use the private key.
   - `bandit14@localhost`: login as `bandit14` on the same machine.
   - `-p 2220`: connect to the custom port used by Bandit.
4. **Read the password from the system path:**
   ```sh
   cat /etc/bandit_pass/bandit14
   ```
## Submit Password via TCP Port
The password can be retrieved by submitting the password of the current level to port `3000` on `localhost`.
```sh
telnet localhost 30000
```
Enter the password of the current level.
## Submit Password over SSL
The password can be retrieved by submitting the current level's password to port `30001` on `localhost` using **SSL/TLS** encryption.
```sh
openssl s_client -connect localhost:30001
```
Enter the password of the current level.
## Finding a port with a server listening in a given range
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000.
### **Steps to Solve**
1. **Scan for open ports in the range:**
   ```sh
   nmap -sV -T4 -31000-32000 localhost
   ```
2. **Connect to the correct port:**
   ```sh
   openssl s_client -connect localhost:31790 -ign_eof
   ```
## Using a private RSA key
Login into the next level using a private RSA key
### **Steps to solve**
1. **Store the key in a tmp file:**
   ```sh
   mkdir /tmp/lev17
   cd /tmp/lev17
   nano private.key
   ```
2. **Change permission on the file to keep it private:**
   ```sh
   chmod 700 private.key
   ```
3. **Log in to the next level:**
   ```sh
   ssh bandit17@localhost -i private.key -p 2220
   ```
## Finding Line change between two files
```sh
diff passwords.old passwords.new
```
Line starting with `>` is the password.
## Reading a file without logging in
The **.bashrc** file has been modified which logs you out when you log in with ssh. Password is in a file named **readme**.
```sh
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```
## Using setuid binary
The password for this level can be accessed only by the permission of the owner of the file: **bandit20**.
```sh
./bandit20-do
./bandit20-do cat /etc/bandit_pass/bandit20
```
**bandit20-do** executes cat as **bandit20**.
## Socket Setuid Privilege Escalation
### **Steps to Solve**
1. **Start a listener:**
   ```bash
   nc -lvp 31337
   ```
2. **In a second SSH session:**
   ```bash
   ./suconnect 31337
   ```
3. **Send Bandit20's password in the listener**.
## Retrieve the password for a cronjob
The cronjob is running automatically on the system.
### **Steps to Solve**
1. **Check for cron jobs:**
   ```bash
   ls -l /etc/cron.d
   ```
2. **Identify relevant cron job:**
   ```bash
   cat /etc/cron.d/cronjob_bandit22
   ```
3. **Inspect the executed script:**
   ```bash
   cat /usr/bin/cronjob_bandit22.sh
   ```
4. **Retrieve the password:**
   ```bash
   cat /tmp/bandit22_password
   ```
## Cron Job Hash-Based Filename Retrieval
The cron job for Bandit23 copies the password file to `/tmp/` using a **dynamic, hash-based filename**.
```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```
Retrieve the password
```bash
cat /tmp/output_from_previous_command
```
## Privilege Escalation via Writable Cron Directory
### **Steps to Solve**
1. **Creating file for a custom script:**
   ```bash
   nano /tmp/myscript.sh
   ```
2. **Custom Script:**
   ```bash
   #!/bin/bash
   cat /etc/bandit_pass/bandit24 > /tmp/bandit24_pass
   chmod +x /tmp/myscript.sh
   ```
3. **Move it to the executable directory:**
   ```bash
   mv /tmp/myscript.sh /var/spool/bandit24/foo/myscript.sh
   ```
4. **Retrieve the password after 2 minutes:**
   ```bash
   cat /tmp/bandit24_pass
   ```
## Brute Forcing a 4 digit PIN
The lab wants you to enter the password with a four digit pin.
### **Steps to Solve**
1. **To create a directory for the script:**
   ```bash
   mkdir /tmp/brute
   ```
2. **Writing the custom script:**
   ```bash
   nano script.sh
   #!/bin/bash
   for i in {0..9}{0..9}{0..9}{0..9}
   do
   echo "password $i" >> list.txt
   done
   ```
3. **Executing the script:**
   ```sh
   chmod +x script.sh
   ./script.sh
   ```
4. **Brute Forcing:**
   ```sh
   cat list.txt | nc localhost 30002
   ```
## Escaping /usr/bin/bash shell to /bin/bash
### **Steps to Solve**
1. Check /etc/passwd
   ```sh
   grep bandit26 /etc/passwd
   ```
   ```ruby
   bandit26:x:11026:11026::/home/bandit26:/usr/bin/showtext
   ```
2. Retrieve the private key for bandit26
   ```sh
   chmod 600 sshkey.private
   ssh bandit26@bandit.labs.overthewire.org -p 2220 -i sshkey.private
   ```
3. Try resizing the terminal window in order to stop the showtext.
4. Escaping showtext
   ```sh
   v
   ```
   Then try one of these
   ```sh
   :!bash
   ```
   or
   ```sh
   :!sh
   ```
   or
   ```sh
   :!set shell=/bin/bash
   :shell
   ```
   or
   ```sh
   :!/bin/bash
   ```
5. Retrieving the password
   ```sh
   ./bandit27-do cat /etc/bandit_pass/bandit27
   ```
## Cloining a Repo
To retrieve the password the a repo has to be cloned.
### **Steps to Solve**
1. **Create a temporary SSH config in /tmp to force port 2220:**
   ```sh
   echo 'Host localhost
      Port 2220
   ' > /tmp/ssh_config
   ```
2. **Clone using the SSH config:**
   ```sh
   GIT_SSH_COMMAND="ssh -F /tmp/ssh_config" git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
   ```
3. **Enter bandit27's password.**
4. **Read the password:**
   ```sh
   ls
   cat README
   ```
## Using Git Log and checkout
The password in the REAME.md file of this repo needs to be accessed by using logs as the password has been removed by commits.
### **Steps to Solve**
1. **Move into the repo:**
   ```sh
   cd repo
   ```
2. **Check the git logs:**
   ```sh
   git log
   ```
3. **Use commits to find the password:**
   ```sh
   git checkout (commit in the log)
   ```
4. **Read the file:**
   ```sh
   cat README.md
   ```
