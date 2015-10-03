## Pacemaker

Note: Pacemaker can come in here to bail us out.


### High availability cluster manager

Note: The Pacemaker stack is the default Linux high-availability
stack, and one of its qualities is that it is...


### Application agnostic

Note: ... application agnostic, so that effectively it can be made to
manage any cluster resource in a highly-available fashion.


Can manage
## LXC
through
### libvirt
or
### native resource agent

Note: Pacemaker offers support for managing LXC containers not in one,
but two ways:

- via libvirt, using the `VirtualDomain` resource agent, or
- via a native `lxc` resource agent, using the `lxc` userspace
  toolchain.


Can manage
## filesystems

Note: Pacemaker is perfectly capable of managing filesystem mounts as
well, including OverlayFS mounts (and of course, mounts for regular
local filesystems).


Can manage
## storage
& data replication

Note: and finally, we can also use Pacemaker to enable and disable
access to storage and data replication facilities.

So how can we use all that to our advantage?


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


<!-- .slide: data-background-iframe="http://localhost:4200/" data-background-size="contain" -->

Note: This is a simple failover. We just put one node in standby,
watch what it's doing, and taking it out of standby again.
