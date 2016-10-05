## LXC HA in 2015


### LXC
### + OverlayFS
### + APT
### + Pacemaker
--------
## = Awesome


<!-- .slide: data-background-image="images/stack.svg" data-background-size="contain" -->

Note: We use two hosts which are identically configured as far as
packages go. Then we define one DRBD device for every container that
we run, and that device gets a Pacemaker-managed filesystem. Pacemaker
also manages the overlay mounts and the actual containers.

So that means that on a host that is in standby (doesn't run any
cluster resources), we can simply run `apt-get dist-upgrade`. That
installs all pending patches and security fixes, all while leaving the
existing containers untouched. Then we fail everything over. If
anything goes wrong, we still have the original system that we can
fail back to. Otherwise, we just repeat the same process on the other
box.

We can nicely automate this with `Dpkg::Pre-Invoke` and
`Dpkg::Post-Invoke` in our APT conf, and initiate all package
installations and updates from Ansible. You could also go all the way
and auto-update with unattended-upgrades, but there is a tiny chance
that APT could get invoked within the failover window on the other
node. So the `Pre-Invoke` check would have to be a little more
involved.

