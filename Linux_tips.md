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
