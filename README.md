# RonR-RaspberryPi-image-utils

**NB** scruss is *not* the author or maintainer of these files (same goes for seamusdemora, who assumed maintenance for this repo from scruss). Please take up any issues or questions in the [Image File Utilities](https://forums.raspberrypi.com/viewtopic.php?t=332000) thread of the Raspberry Pi Forums site. ***IOW: This is a file repository only; no support is available here.***

Files here are a toolset to create a backup of a running RPi OS to an SD card image file.

The files here are copies of those posted in the [Raspberry Pi Forums](https://www.raspberrypi.org/forums/index.php). The file attachments in this forum don't seem to be persistent, so this repo was created by user scruss (and now maintained by seamus) *in an effort* to ensure a current working copy of *`image-utils`* is always available through `git`.

The links below will take you to archives of the original post/attachment on the [Internet Archive](https://archive.org/). Please note that the original attachment link will get the original `image-util` scripts - which are 2019 vintage:

* article: https://web.archive.org/web/20190824163430/https://www.raspberrypi.org/forums/viewtopic.php?t=247568

* attachment: https://web.archive.org/web/20190824162104/https://www.raspberrypi.org/forums/download/file.php?id=31366&sid=107ba04af18e19ad587c5bcf8ebacd38  

> *NOTE: scruss' original README updated on Feb 12, 2023.* 



## How Do I Use This Repo?

This repo was created to make a current copy of the RPi `image-utils` toolset available through `git`. There are many resources available online describing the use of `git`, so these instructions are minimal. If you have questions, please consult a tutorial of your own choosing. The instructions below reflect using `bash` from a Raspberry Pi OS terminal or SSH, and assume that `git` is installed: 

### 1. clone the repo
```bash
$ pwd
/home/pi
$ mkdir gitrepos     # or use a directory of your own choosing
$ cd gitrepos
$ git clone https://github.com/seamusdemora/RonR-RPi-image-utils.git
```
#### which should yield something like this:
>Cloning into 'RonR-RPi-image-utils'...  
remote: Enumerating objects: 112, done.  
remote: Counting objects: 100% (45/45), done.  
remote: Compressing objects: 100% (28/28), done.  
remote: Total 112 (delta 28), reused 26 (delta 16), pack-reused 67  
Receiving objects: 100% (112/112), 41.92 KiB | 1.45 MiB/s, done.  
Resolving deltas: 100% (64/64), done.  

### 2. take a look around & verify clone operation succeeded:
```bash
$ ls -la RonR-RPi-image-utils
```
#### which should yield something like this:
>total 76  
drwxr-xr-x 4 pi pi  4096 Feb 13 01:25 .  
drwxr-xr-x 3 pi pi  4096 Feb 13 01:25 ..  
drwxr-xr-x 2 pi pi  4096 Feb 13 01:25 deprecated  
drwxr-xr-x 8 pi pi  4096 Feb 13 01:25 .git  
-rw-r--r-- 1 pi pi 13855 Feb 13 01:25 image-backup  
-rw-r--r-- 1 pi pi  1534 Feb 13 01:25 image-check  
-rw-r--r-- 1 pi pi  3759 Feb 13 01:25 image-chroot  
-rw-r--r-- 1 pi pi  3458 Feb 13 01:25 image-compare  
-rw-r--r-- 1 pi pi  3152 Feb 13 01:25 image-info  
-rw-r--r-- 1 pi pi  1667 Feb 13 01:25 image-mount  
-rw-r--r-- 1 pi pi  5639 Feb 13 01:25 image-set-partuuid  
-rw-r--r-- 1 pi pi  4150 Feb 13 01:25 image-shrink  
-rw-r--r-- 1 pi pi  3478 Feb 13 01:25 README.md  
-rw-r--r-- 1 pi pi  4035 Feb 13 01:25 README.txt  

### 3. keep your clone synced to stay current:
```bash
$ cd RonR-RPi-image-utils
$ git config pull.rebase false    # this only needs to be done one time (the first time)
$ git pull                        # all subsequent updates require only this command 
```

## Staging & Usage (Optional) 

Once you have all of the `inage-utils` files in your local git repository, you may find it's worthwhile to keep working copies in your PATH. This makes it easier to use them either directly from the command line, or (for example) in an `cron` job. Here's one way to do this:

```bash
$ pwd
/home/pi/gitrepos/RonR-RPi-image-utils
$ sudo cp -uv --preserve=timestamps image-* /usr/local/sbin/
$ sudo chmod 755 /usr/local/sbin/image-* 
```

## Creating vs. Updating .img backups (Optional)

Refer to the [Image File Utilities](https://forums.raspberrypi.com/viewtopic.php?t=332000) thread of the Raspberry Pi Forums site for documentation & support. This is offered only as an illustration/example:

### Create the .img backup:

To create a ***NEW*** image backup, use the `sudo image-backup`  command; you will be prompted for inputs. The ones I typically use are shown below immediately following the question mark `?`: 

```
$ sudo image-backup

Image file to create? /mnt/SynologyNAS/rpi_share/raspberrypi3b/20230212_Pi3B_imagebackup.img

Initial image file ROOT filesystem size (MB) [2317]? 2400

Added space for incremental updates after shrinking (MB) [0]? 200

Create /mnt/SynologyNAS/rpi_share/raspberrypi3b/20230212_Pi3B_imagebackup.img (y/n)?y
```

This will take a few minutes depending on your model Pi, the size of your file system & other variables. Upon completion, you should find the image file you specified in your answer to the first question. This image file contains everything exactly as it was in your file system at the time of the backup. This image file may be written to an SD card, or `mount`-ed as another file system on your RPi. 

### Update an existing .img backup:

To **update** the image file you have created is even easier; `sudo image-backup <IMG_TO_UPDT>`, or:

```bash
$ sudo image-backup /mnt/SynologyNAS/rpi_share/raspberrypi3b/20230212_Pi3B_imagebackup.img
```

In other words, simply add the URL of the .img file you wish to update to the basic `sudo image-backup` command.



## RonR's latest README.txt (with a tiny bit of Markdown added) follows:

---------------------------------------------------------------------
## image-backup:

Usage: image-backup [options] [pathto/imagefile for incremental backup]
-h,--help       This usage description
-i,--initial    pathto/filename of image file [,inital size MB [,added space for incremental MB]]
-n,--noexpand   Do not expand filesystem on first run after restore
-o,--options    Additional rsync options (comma separated)
-u,--ubuntu     Ubuntu (Deprecated)
-x,--extract    Extract image from NOOBS (force BOOT partition to -01 / ROOT partition to -02)"

image-backup creates a backup of a running Raspbian system to a standard 'raw' image file that can be written to an SD card or a USB device with Etcher, imageUSB, etc. It will also perform incremental updates to an existing backup image file.

Running image-backup with no incremental backup file parameter will create an initial/full backup. If no -i/--initial option is included, image-backup will interactively prompt for an "Initial image file ROOT filesystem size (MB)", which can be any size as long as (1) it's large enough to hold the data contained on the device being backed up and (2) that amount of space is available on the destination device.  image-backup will also prompt for an "Added space for incremental updates after shrinking (MB)" which will be added to the image file size after the full backup completes and the image file has been shrunk to the smallest size possible.  The resulting image file will auto-expand the first time it's executed unless the -n/--noexpand option is included.

Running image-backup with a parameter of an existing image filename will incrementally update that image file.  The -i/--initial option is ignored on incremental backups.

The -o/--options option permits including additional rsync options (comma separated).

Examples:

image-backup

image-backup /media/backup.img

image-backup --initial /media/backup.img,,5000 --noexpand --options --exclude-from=/home/pi/exclude.txt,--delete-excluded


## Image-check:

Usage: image-check imagefile [W95|Linux]

where W95 checks the BOOT partition and Linux checks the ROOT partition.  If neither is specified, Linux is assumed.

image-check will check the integrity of a standard 'raw' image file.


## image-chroot:

Usage: image-backup [options] pathto/imagefile
-h,--help       This usage description
-u,--ubuntu     Ubuntu (Deprecated)

image-chroot performs a linux 'chroot' to an image file.  The current user will be 'root' and the current directory will be '/' in the image file.  The host's root filesystem will be available at /host-root-fs.  Use exit or ^D to terminate chroot.


## image-compare:

Usage: image-backup [options] pathto/imagefile
-h,--help       This usage description
-o,--options    Additional rsync options (comma separated)
-u,--ubuntu     Ubuntu (Deprecated)

image-compare compares a running Raspbian system to an existing standard 'raw' image file and displays the incremental changes that image-backup would perform if run.


## image-info:

Usage: image-backup [options] pathto/imagefile
-h,--help       This usage description
-u,--ubuntu     Ubuntu (Deprecated)

image-info displays information about a standard 'raw' image file.


## image-mount:

Usage: image-mount imagefile mountpoint [W95|Linux]

where W95 mounts the BOOT partition and Linux mounts the ROOT partition.  If neither is specified, Linux is assumed.

image-mount mounts a standard 'raw' image file to allow it to be read or written as if it were a device.


## image-set-partuuid:

Usage: image-set-partuuid imagefile [ hhhhhhhh-02 | hhhhhhhh-hhhh-hhhh-hhhh-hhhhhhhhhhhh | random ]"

If no partuuid is specified, the current ROOT partuuid will be displayed.

image-set-partuuid sets the ROOT partition PARTUUID value of a standard 'raw' image file.


## image-shrink:

Usage: image-shrink imagefile [Additional MB]

where Additional MB is an optional additional amount of free space to be added.

image-shrink shrinks a standard 'raw' image file to its smallest possible size (plus an optional additional amount of free space).

<!--- 
You can hide shit in here  :)   LOL 

Following is an older version of README.txt
--->
<!---
## image-backup:

`image-backup` creates a backup of a running Raspbian system to a standard 'raw' image file that can be written to an SD card or a USB device card with Etcher, imageUSB, etc. It will also perform incremental updates to an existing backup image file.

Running image-backup with no parameters will create a full backup. To create the smallest possible image, specify an Image ROOT filesystem size of 0 to determine the minimum allowed size. If you plan to incrementally update the image file, specify a considerably larger size to allow for additional growth.

Running image-backup with a parameter of an existing image filename will incrementally update that image file.

## image-check:

`image-check` will check the integrity of a standard 'raw' image file.  Usage is:

    image-check imagefile [W95|Linux]

where *W95* checks the BOOT partition and *Linux* checks the ROOT partition.  If neither is specified, *Linux* is assumed.


## image-compare:

`image-compare` compares a running Raspbian system to an existing standard 'raw' image file and displays the incremental changes that image-backup would perform if run.  Usage is:

    image-compare [imagefile]


## image-mount:

`image-mount` mounts a standard 'raw' image file to allow it to be read or written as if it were a device.  Usage is:

    image-mount imagefile mountpoint [W95|Linux]

where *W95* mounts the BOOT partition and *Linux* mounts the ROOT partition.  If neither is specified, *Linux* is assumed.


## image-set-ptuuid:

`image-set-ptuuid` sets the Partition Table UUID value of a standard 'raw' image file.  Usage is:

    image-set-ptuuid imagefile ptuuid

where *ptuuid* is 8 hex digits


## image-shrink:

`image-shrink` shrinks a standard 'raw' image file to its smallest possible size (plus an optional additional amount of free space).  Usage is:

    image-shrink imagefile [Additional MB]

where *Additional MB* is an additional amount of free space to be added. 

--->
