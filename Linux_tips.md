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
