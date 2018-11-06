# Ansible Role: shadowsocks-libev

[![Build Status](https://travis-ci.org/meteorlad/ansible-role-shadowsocks-libev.svg?branch=master)](https://travis-ci.org/meteorlad/ansible-role-shadowsocks-libev)

Install [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev) via Ansible.

## Features

- Install or upgrade shadowsocks-libev version easily
- Tuning `sysctl` automatically for better performance
- Detect `tcp_fastopen` support
- Support init startup script

## Requirements

This role requires Ansible 1.6 or higher and platform requirements are listed in the metadata file.

## Dependencies

None

## Example Playbooks

Install shadowsocks-libev with custom location (default: `/etc/shadowsocks-libev`):

```yaml
- hosts: servers
  roles:
    - { role: sparanoid.shadowsocks-libev, shadowsocks_home: /etc/shadowsocks-libev }
```

Install shadowsocks-libev with different encryption (default: `aes-256-gcm`):

```yaml
- hosts: servers
  roles:
    - { role: sparanoid.shadowsocks-libev, shadowsocks_config_method: salsa20 }
```

Install shadowsocks-libev with different server port (default: `443`):

```yaml
- hosts: servers
  roles:
    - { role: sparanoid.shadowsocks-libev, shadowsocks_config_server_port: 9999 }
```

Install shadowsocks-libev without tuning `sysctl` (default: `shadowsocks_sysctl_tweak: true`):

```yaml
- hosts: servers
  roles:
    - { role: sparanoid.shadowsocks-libev, shadowsocks_sysctl_tweak: false }
```

Install shadowsocks-libev without shadowsocks/simple-obfs support (default: `shadowsocks_obfs: true`):

```yaml
- hosts: servers
  roles:
    - { role: sparanoid.shadowsocks-libev, shadowsocks_obfs: false }
```

Install shadowsocks-libev using custom specified password (default: `apn!proxy!ss!ftw!`):

```yaml
- hosts: servers
  roles:
    - { role: sparanoid.shadowsocks-libev, shadowsocks_password: "myFancy@Passwd!" }
```

You can also define password in command line:

```shell
$ ansible-playbook shadowsocks-servers.yml --extra-vars "shadowsocks_password=myFancy@Passwd!"
```

## Todos

- [x] init.d script
- [x] Tuning `sysctl` support
- [x] Detect `tcp_fastopen`
- [ ] Multiple users support
- [ ] Distro support
  - [x] EL
    - [x] Better init.d script for 7
  - [ ] Debian
  - [ ] Ubuntu

## FAQ

依赖于 epel-release源，在部署的时候需要先执行epel源

启动的时候会报错，这个时候需要手动做下软连接 `ln -s /usr/lib64/libmbedcrypto.so.2.7.6 /usr/lib64/libmbedcrypto.so.1`，如下是报错异常：

```log
[root@us-ss-node01 lib64]# tail -f /var/log/messages
Nov  5 23:13:00 susan systemd: Starting Shadowsocks-libev Default Server Service...
Nov  5 23:13:00 susan ss-server: /usr/bin/ss-server: error while loading shared libraries: libmbedcrypto.so.1: cannot open shared object file: No such file or directory
Nov  5 23:13:00 susan systemd: shadowsocks-libev.service: main process exited, code=exited, status=127/n/a
Nov  5 23:13:00 susan systemd: Unit shadowsocks-libev.service entered failed state.
Nov  5 23:13:00 susan systemd: shadowsocks-libev.service failed

```

## License

GPLv3

## Author Information

**Meteor Liu**