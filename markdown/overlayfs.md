## OverlayFS


An in-kernel
### filesystem
merged in Linux 3.18

Note: 3.18 landed in November 2014. OverlayFS was previously already
available in Ubuntu 14.04.


### Union mount
filesystem
Note: OverlayFS isn't the *only* union mount filesystem in existence,
among its predecessors were UFS and AUFS. OverlayFS is just the first
one that made it upstream.


#### OverlayFS
## &uarr;
#### upperdir
## &uarr;
#### lowerdir
Note: In OverlayFS we always deal with three directories directly, and
one more is internal to OverlayFS' workings.

The **OverlayFS** is the union mount at the top of the stack.

The **lowerdir** is our template or baseline directory.

The **upperdir** is a directory that stores all the files by which the
union mount differs from the lowerdir.

And finally, there's also a **workdir** that OverlayFS uses
internally, but that is not exposed to users.

In **reads**, all data that exists in the upperdir is served from
there, and anything that only exists in the lowerdir transparently
passes through.


#### OverlayFS
## &darr;
#### upperdir
## &times;
#### lowerdir

Note:
**Writes**, in contrast, never hit the lower directory, they always go
to the upper directory.

What this means is that our template lowerdir stays pristine, it is
only the upperdir that the OverlayFS mount physically modifies.


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


# How
does this
# help
with application containers?

Note: So this means that on any host we can have our host filesystem
as the template for any number of containers using the same
OS. Anything that is installed on the host, all containers inherit,
but they all only run exactly the services they need. And if there is
any piece of software that we need to update, we just do so on our
host, and as soon as we remount the overlays and restart each
container, it is immediately updated.

But that still leaves us with two problems:
- We have a window of undefined behavior while the upgrade is underway
  and containers are still running.
- Having to do this manually doesn't scale; we have to find an
  appropriate means of automating it.