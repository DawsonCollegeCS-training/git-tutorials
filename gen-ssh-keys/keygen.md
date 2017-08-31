# Generate an SSH key 

## What is this?
This file contains instructions for generating ssh keys on a host Windows must have git-bash or other installed, 
Linux and OS X use a terminal window.

## Instructions
1. Open a terminal window (ex git-bash)
2. Display your home directory
```{.bash}
$ echo $HOME
```
3. Make your home directory your current working direcotry. 
```{.bash}
# cd to your home directory
$ cd 
```
4. Generate your keys
```{.bash}
ssh-keygen -t rsa -b 4096 -C "youremail@address.here"
# For security, provide a passphrase.
# Press enter to save the key in $HOME/.ssh/id_rsa
# check the key gen
$ ls -la .ssh/
total 37
drwxr-xr-x 1 owner 197121    0 Aug 11 17:10 ./
drwxr-xr-x 1 owner 197121    0 Aug 30 13:51 ../
-rw-r--r-- 1 owner 197121 1679 Nov 29  2016 id_rsa
-rw-r--r-- 1 owner 197121  403 Nov 29  2016 id_rsa.pub
-rw-r--r-- 1 owner 197121 1163 Aug 11 17:04 known_hosts
# look at the public key
$ cat .ssh/id_rsa.pub
```
