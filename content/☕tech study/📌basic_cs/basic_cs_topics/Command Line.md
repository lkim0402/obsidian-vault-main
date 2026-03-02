> 
- Most OS are structured in a tree-like hierarchy
	- The folder parent-child relationship is modeled like a tree with a root directory.
- ~(home)
	- the default directory upon opening a new terminal window

Useful terminal commands (Linux Bash commands)
```bash
ls 
ls Documents/vault #can be used outside of current dir
ls -a #shows hidden files
pwd # print working directory

#cd = change directory
cd # reset
cd (folder)
cd .. #go backwards

touch new_file1.txt new_file2.txt #makes new file(s) 
touch Desktop/new_file.py #can state directory
code new_file.txt #vs code
nano new_file.txt
vim new_file.txt

mkdir new_folder1 new_folder2 #make new folder(s)

rm test1.txt test2.txt #deleting file(s) completely
rm -rf new_folder #deleting folders/directories
#r: recursive, f: force (prevents warnings, optional)
rmdir new_folder #deletes only EMPTY directories
clear

# Moving/renaming things
# source and destination
mv cats.txt animals.txt #renames cats.txt to animals.txt
mv cats.txt ../ #moves cats.txt to parent
mv folder1/cat1.jpg folder2/cat2.jpg

#opening current directory
xdg-open . #linux bash
open . #mac
start . #windows

#opens manual 
man <command>
man ls
```