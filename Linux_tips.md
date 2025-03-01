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
