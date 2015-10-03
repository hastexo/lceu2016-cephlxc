## OverlayFS


An in-kernel
### filesystem
merged in Linux 3.18

Note: 3.18 landed in November 2014. OverlayFS was previously already
available in Ubuntu 14.04.


### Union mount
filesystem


#### OverlayFS
## &uarr;
#### upperdir
## &uarr;
#### lowerdir


#### OverlayFS
## &darr;
#### upperdir
## &times;
#### lowerdir


<!-- .slide: data-background-iframe="http://localhost:4200/" data-background-size="contain" -->

Note:
- ls -lR lower, upper
- mount -t overlayfs -o lowerdir=$PWD/lower,upperdir=$PWD/upper,workdir=$PWD/work none $PWD/overlay
- Explain contents
- cd overlay
- mkdir blatch
- touch blatch/blatchfile
- echo hello > bar/barfile
- ls -lR lower, upper, overlay
- umount overlayfs
- Clean out upper
- Mount overlayfs from /
- ls overlay
- chroot overlay
- ls
- ls /tmp
- ls /root
- cd ..
- mkdir upper/{root,tmp}
- setfattr -n trusted.overlay.opaque -v y upper/{root,tmp}
- mount -o remount $PWD/overlay
- chroot again
- ps -AHfww
- netstat -lntp
- exit
- lxc-start -n bash -d
- lxc-attach -n bash
- ps -AHfww
- netstat -lntp