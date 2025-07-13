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
