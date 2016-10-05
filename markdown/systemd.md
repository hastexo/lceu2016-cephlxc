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
