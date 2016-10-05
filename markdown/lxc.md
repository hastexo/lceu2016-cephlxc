# LXC
## containers

Note: LXC containers are a means of cleverly making use of Linux
kernel features, such as..


## Lightweight virtualization
### cgroups
### namespaces

Note: ... network and process namespaces, and cgroups, to isolate
processes from one another. What LXC does is basically set up
namespace and cgroup scaffolding for a particular process, and then
start that process, which becomes PID 1 in the container.


# Two
### container types

Note: Based on **what* exactly the process is that is being started
as the container's PID 1, we can distinguish between two different
container types:


## System
### containers

Note: A **system** container is one where the container manager (like
LXC) starts an init process, like SysV init or upstart or
systemd. This is the most common use case. However, this is by no
means required.


## Application
### containers

Note: In contrast, an **application** container is one where LXC
starts any other binary, like, say, MySQL or Apache or lighttpd or
HAproxy or whatever your conntainer is meant to run.
