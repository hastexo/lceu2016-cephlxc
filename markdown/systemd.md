## Systemd
and
## Fleet


# LXC
with
## systemd


```ini
[Unit]
Description=Container foo service

[Service]
Type=forking
ExecStart=/usr/bin/lxc-start -d -n foo
ExecStop=/usr/bin/lxc-stop -n foo
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
Actually, why not? <!-- .element class="fragment" -->


# LXC
with
## fleet


```ini
[Unit]
Description=Container {{ item }} service

[Service]
Type=forking
ExecStart=/usr/bin/lxc-start -d -n {{ item }}
ExecStop=/usr/bin/lxc-stop -n {{ item }}
RemainAfterExit=yes

[X-Fleet]
Conflicts=lxc-{{ item }}@*.service
```


<!-- .slide: data-background-iframe="http://localhost:4200/" data-background-size="contain" -->