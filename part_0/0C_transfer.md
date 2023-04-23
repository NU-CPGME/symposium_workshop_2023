# Data transfer: ssh, ftp, wget, rsync

### August 2022

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 

# Step 1 - ssh

SSH stands for "secure shell" or "secure socket shell". SSH is used to access other computers, like a server, over a network or over the internet. 

To use, you'll need the address of the server, a login name, and usually a password

Example:

```
ssh workshop@tty.sdf.org
``` 

To close a ssh session, type `exit` 
 
 
# Step 2 - ftp and sftp

FTP stands for file transfer protocol. SFTP stands for file transfer protocol over ssh. These methods are used to transfer files back and forth over the internet. This is easiest to do using a program like [Cyberduck](https://cyberduck.io/download/) or [FileZilla](https://filezilla-project.org/), but can be done by command line as well. 


# Step 3 - wget

This is used to download files from internet links (HTTP, HTTPS, FTP or FTPS). Files will be downloaded into whatever directory you are currently in.

Example (download a text file from the internet):

```Shell
wget https://www.w3.org/TR/PNG/iso_8859-1.txt
```

# Step 4 - rsync

Rsync stands for "remote sync". This utility is useful for moving large amounts of files as well as backing up directories. Can be used for copying files or directories on the same computer, but excels at moving large amounts of files between servers. Because the "archive" option of rsync tries to make a perfect copy of the files being transferred, if a transfer is interrupted due to a lost connection, it can resume where it left off if you give the same command again after reconnecting.

A common method for copying to or from a server:

```
rsync -a -L --progress <source> <destination>
```

* `-a` Archive option. 
* `-L` Copy files that symlinks are pointing to, not the symlink pointer
* `--progress` Shows the progress of the transfer


---

# [Back to table of contents](../README.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.