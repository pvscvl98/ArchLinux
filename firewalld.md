# Linux Firewall using [nftables](https://wiki.archlinux.org/title/Nftables) + [firewalld](https://wiki.archlinux.org/title/Firewalld)

```
sudo pacman -S nftables firewalld
```

```
sudo systemctl enable --now firewalld
```

## firewall-cmd (CLI)

info on runtime configuration
```
sudo firewall-cmd --list-all
```
for information on default zones, consult:
```
man 5 firewalld.zones
```

list services of the current runtime zone
```
sudo firewall-cmd --list-service
```

set default zone
```
sudo firewall-cmd --set-default-zone=home
```

list all available services
```
sudo firewall-cmd --get-services
```

query information about a service
```
sudo firewall-cmd --info-service service_name
```

add a service to the runtime zone
```
sudo firewall-cmd --add-service service_name
```

add a service to the permanent zone
```
sudo firewall-cmd --add-service service_name --permanent
```

remove a service from runtime zone
```
sudo firewall-cmd --remove-service service_name
```

remove a service from permanent zone
```
sudo firewall-cmd --remove-service service_name --permanent
```

## firewall-config (GUI)

```
sudo firewall-config
```

___

# Firewalld
https://firewalld.org/documentation/
https://firewalld.org/documentation/howto/
https://wiki.archlinux.org/title/firewalld
___
You can control the firewall rules with the `firewall-cmd` console utility.
`firewall-offline-cmd` utility can be used to configure when firewalld is not running. It features similar syntax to `firewall-cmd`.
___
##### Runtime Configuration
The runtime configuration is the actual effective configuration and applied to the firewall in the kernel. At firewalld service start the permanent configuration becomes the runtime configuration. Changes in the runtime configuration are not automatically saved to the permanent configuration.
The runtime configuration will be lost with a firewalld service stop. A firewalld reload will replace the runtime configuration by the permanent configuration. Changed zone bindings will be restored after the reload.

#### Permanent Configuration
The permanent configuration is stored in configuration files and will be loaded and become new runtime configuration with every machine boot or service reload/restart.

#### Runtime to Permanent
The runtime configuration can also be used to create a firewall setup that fits the needs. When it is complete and working it can be migrated with the runtime to permanent migration. It is available in `firewall-config` (GUI) and `firewall-cmd` (CLI).

```bash
firewall-cmd --runtime-to-permanent
```
If the firewall setup is not working, a simple firewalld reload/restart will reapply the working permanent configuration.
___
Default / Fallback configuration: `/usr/lib/firewalld/`
System specific configuration: `/etc/firewalld/`
___
To get the firewalld state with firewall-cmd, use the following command:
```bash
firewall-cmd --state
```
```bash
systemctl status firewalld
```

```bash
firewall-cmd --get-active-zones
```

print `information` about a `zone`:
```bash
firewall-cmd --info-zone=public
```
```bash
firewall-cmd --info-zone=public --permanent
```

allow a port in a zone of the runtime configuration:
```bash
firewall-cmd --zone=public --add-port=1337/tcp
```
allow a port in a zone of the permanent configuration:
```bash
firewall-cmd --zone=public --add-port=1337/tcp --permanent
```

print `information` about a `service`:
```bash
firewall-cmd --info-service=https
```
```bash
firewall-cmd --info-service=https --permanent
```

allow a service in the runtime configuration:
```bash
firewall-cmd --zone=public --add-service=https
```
allow a service in the permanent configuration:
```bash
firewall-cmd --zone=public --add-service=https --permanent
```

allow a port in a service of the runtime configuration:
```bash
firewall-cmd --service=https --add-port=16443/tcp
```
allow a port in a service of the permanent configuration:
```bash
firewall-cmd --service=https --add-port=16443/tcp --permanent
```

___
edit default permanent service file (opsec):
```bash
vim /usr/lib/firewalld/services/https.xml
```
edit default permanent zone file (opsec):
```bash
vim /usr/lib/firewalld/zones/public.xml
```

___
reload firewall and keep state information:
```bash
firewall-cmd --reload
```

reload firewall and lose state information, permanent configuration will become new runtime configuration.
```bash
firewall-cmd --complete-reload
```

___
to add a new and empty service, use the `--new-service` altogether with the `--permanent` option:
```bash
firewall-cmd --permanent --new-service=myservice
```

configure/edit the service:
```bash
firewall-cmd --permanent --service=myservice --set-description=description
firewall-cmd --permanent --service=myservice --set-short=description
firewall-cmd --permanent --service=myservice --add-port=portid[-portid]/protocol
firewall-cmd --permanent --service=myservice --add-protocol=protocol
firewall-cmd --permanent --service=myservice --add-source-port=portid[-portid]/protocol
firewall-cmd --permanent --service=myservice --set-destination=ipv:address[/mask]
```
___
