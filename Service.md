## Services

Below is a simplified overview of the entire Linux boot and startup process:

1. The system powers up. 1 The BIOS does minimal hardware initialization and hands over control to the boot loader.
2. The boot loader calls the kernel.
3. The kernel loads an initial RAM disk that loads the system drives and then looks for the root file system.
4. Once the kernel is set up, it begins the systemd initialization system.
5. systemd takes over and continues to mount the host’s file systems and start services.

In systemd, the target of most actions are “units”, which are resources that systemd knows how to manage. Units are categorized by the type of resource they represent and they are defined with files known as unit files. The type of each unit can be inferred from the suffix on the end of the file.

For service management tasks, the target unit will be service units, which have unit files with a suffix of .service. However, for most service management commands, you can actually leave off the .service suffix, as systemd is smart enough to know that you probably want to operate on a service when using service management commands.

```bash
# enable a service 
$ systemctl enable application.service
# disable a service
$ sudo systemctl disable application.service

# start a service
$ systemctl start application.service
# systemd knows to look for *.service files for service management commands
$ systemctl start application
# stop a service
$ sudo systemctl stop application.service
# restart a service
$ sudo systemctl restart application.service
# reload configuration
$ sudo systemctl reload application.service

# check service status
$ systemctl status application.service
$ systemctl is-active application.service
$ systemctl is-enabled application.service
$ systemctl is-failed application.service

# list current units
$ systemctl list-units
# list all services
$ systemctl list-units --type=service
# list all unit files
$ systemctl list-unit-files

# Reload systemd manager configuration
$ systemctl daemon-reload
```
Sample unit file
```
[Unit]
Description=ATD daemon
[Service]
Type=forking
ExecStart=/usr/bin/atd
[Install]
WantedBy=multi-user.target
```
