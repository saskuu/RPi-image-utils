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

```bash
$ pwd
/home/pi
$ mkdir gitrepos					# or use a directory of your own choosing
$ cd gitrepos
$ git clone https://github.com/seamusdemora/RonR-RPi-image-utils.git
  # which should yield something like this:
Cloning into 'RonR-RPi-image-utils'...
remote: Enumerating objects: 112, done.
remote: Counting objects: 100% (45/45), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 112 (delta 28), reused 26 (delta 16), pack-reused 67
Receiving objects: 100% (112/112), 41.92 KiB | 1.45 MiB/s, done.
Resolving deltas: 100% (64/64), done. 

   # take a look around & verify clone operation:
$ ls -la RonR-RPi-image-utils
total 76
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

   # keep your clone synced to stay current:
$ cd RonR-RPi-image-utils
$ git config pull.rebase false		# this only needs to be done one time (the first time)
$ git pull												# all subsequent updates require only this command 
```

## Staging & Usage (Optional) 

Once you have all of the `inage-utils` files in your local git repository, you may find it's worthwhile to keep working copies in your PATH. This makes it easier to use them either directly from the command line, or (for example) in an `cron` job. Here's one way to do this:

```bash
$ pwd
/home/pi/gitrepos/RonR-RPi-image-utils
$ sudo cp image-* /usr/local/sbin/
$ sudo chmod 755 /usr/local/sbin/image-* 
```

## Creating vs. Updating .img backups (Optional)

Refer to the [Image File Utilities](https://forums.raspberrypi.com/viewtopic.php?t=332000) thread of the Raspberry Pi Forums site for documentation & support. This is offered only as an illustration/example:

#### CREATE:

To create a **new** image backup, use the `sudo image-backup`  command; you will be prompted for inputs. The ones I typically use are shown below immediately following the question mark `?`: 

```bash
$ sudo image-backup

Image file to create? /mnt/SynologyNAS/rpi_share/raspberrypi3b/20230212_Pi3B_imagebackup.img

Initial image file ROOT filesystem size (MB) [2317]? 2400

Added space for incremental updates after shrinking (MB) [0]? 200

Create /mnt/SynologyNAS/rpi_share/raspberrypi3b/20230212_Pi3B_imagebackup.img (y/n)?y
```

This will take a few minutes depending on your model Pi, the size of your file system & other variables. Upon completion, you should find the image file you specified in your answer to the first question. This image file contains everything exactly as it was in your file system at the time of the backup. This image file may be written to an SD card, or `mount`-ed as another file system on your RPi. 

#### UPDATE:

To **update** the image file you have created is even easier; `sudo image-backup <IMG_TO_UPDT>`, or:

```bash
$ sudo image-backup /mnt/SynologyNAS/rpi_share/raspberrypi3b/20230212_Pi3B_imagebackup.img
```



## Original README converted to markdown follows:

---------------------------------------------------------------------

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
