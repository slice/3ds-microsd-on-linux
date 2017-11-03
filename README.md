# 3ds-microsd-on-linux
This is a guide detailing how to use 3DS microSD management on Linux.

## Getting started

This command mounts the 3DS's microSD card as a CIFS.

```sh
$ sudo mount.cifs \
	//$3DS_NAME/microSD\
	-o user=$3DS_USER,\
	password=$3DS_PASS,\
	ip=$3DS_LOCALIP,\
	servern=$3DS_NAME,\
	uid=$USER,gid=users,nounix /mnt
```
That command will successfully mount your 3DS's microSD card to `/mnt`. Please note that `sudo` is required.

If the connection has been dropped (for example, the 3DS is not in microSD management), commands like `ls` will freeze.


## Connection loss

A downside of doing this is that connection is very unstable. Even a simple `ls /mnt` will cause a the filesystem to "ghost".

## Filesystem ghosting

If you mount the share, and execute `ls`, it will seem to "empty" out the share for some reason. However, files will **still be writeable.** You just can't see them.

This is proven by the following:

```sh
[/]% ls /mnt
 Directory of /mnt
Total                       0 bytes
Free space        59021819904 bytes (95.4%)

# Listing /mnt (the share.) Note the amount of bytes
# free; "Free space"

[/]% vim /mnt/something.txt

# I open up vim and write something.

[/]% ls /mnt               
 Directory of /mnt
Total                       0 bytes
Free space        59021787136 bytes (95.4%)

# Note that it seems empty...but the Free space has
# DECREASED!
[/]% 
```

The updating of "Free space" seems to be unstable, as I only wrote a few words to `/mnt/something.txt`, and the free space lowered by a lot of bytes. It got lowered by a lot of bytes because I just copied the starter kit to the SD card a few minutes ago, and I'm assuming that the remaining space had only updated just now.
