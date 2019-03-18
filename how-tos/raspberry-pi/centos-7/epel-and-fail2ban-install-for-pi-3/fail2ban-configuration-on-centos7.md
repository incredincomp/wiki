---
description: >-
  Secure your stack! This is pretty much the best way I've found to attempt the
  conundrum of securing my web server.
---

# fail2ban Configuration on CentOS7

## Best Practices

 Fail2ban reads `.conf` configuration files first, then `.local` files override any settings. Because of this, all changes to the configuration are generally done in `.local` files, leaving the `.conf` files untouched.

## Configuring fail2ban.local

1.  `fail2ban.conf` contains the default configuration profile. The default settings will give you a reasonable working setup. If you want to make any changes, it’s best to do it in a separate file, `fail2ban.local`, which overrides `fail2ban.conf`. Rename a copy `fail2ban.conf` to `fail2ban.local`.

```bash
cp /etc/fail2ban/fail2ban.conf /etc/fail2ban/fail2ban.local
```



   2.    From here, you can opt to edit the definitions in `fail2ban.local` to match your desired   configuration. The values that can be changed are:

* `loglevel`: The level of detail that Fail2ban’s logs provide can be set to 1 \(error\), 2 \(warn\), 3 \(info\), or 4 \(debug\).
* `logtarget`: Logs actions into a specific file. The default value of `/var/log/fail2ban.log` puts all logging into the defined file. Alternately, you can change the value to:

  * `STDOUT`: output any data
  * `STDERR`: output any errors
  * `SYSLOG`: message-based logging
  * `FILE`: output to a file

  \`\`

* `socket`: The location of the socket file.
* `pidfile`: The location of the PID file.

## Configure jail.local Settings

By default, fail2ban does not enable ssh for CentOS so we need to make some changes to the `jail.conf` file.

```bash
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
vim /etc/fail2ban/jail.local
```

We first need to change the backend option in jail.local from auto to systemd.

```bash
backend = systemd
```

{% hint style="warning" %}
No jails are enabed by default on CentOS7. 
{% endhint %}

To enable your ssh daemon jail, uncomment the following lines:

```bash
[sshd]
enabled = true
```

### Whitelist IP

To ignore your own IP so that you don't accidentally jail your own self:

```bash
[DEFAULT]

# "ignoreip" can be an IP address, a CIDR mask or a DNS host. Fail2ban will not
# ban a host which matches an address in this list. Several addresses can be
# defined using space separator.
ignoreip = 127.0.0.1/8 123.45.67.89
```

{% hint style="info" %}
 If you wish to whitelist IPs only for certain jails, this can be done with the `fail2ban-client` command. Replace `JAIL` with the name of your jail, and `123.45.67.89` with the IP you wish to whitelist.
{% endhint %}

```bash
fail2ban-client set JAIL addignoreip 123.45.67.89
```

### Ban Times and Retry Amount

```bash
# "bantime" is the number of seconds that a host is banned.
bantime  = 600

# A host is banned if it has generated "maxretry" during the last "findtime"
# seconds.
findtime = 600
maxretry = 3
```



* `bantime`: The length of time in seconds for which an IP is banned. If set to a negative number, the ban will be permanent. The default value of `600` is set to ban an IP for a 10-minute duration.
* `findtime`: The length of time between login attempts before a ban is set. For example, if Fail2ban is set to ban an IP after five \(5\) failed log-in attempts, those 5 attempts must occur within the set 10-minute `findtime` limit. The `findtime` value should be a set number of seconds.
* `maxretry`: How many attempts can be made to access the server from a single IP before a ban is imposed. The default is set to 3.

### Other Jail Config Settings

```bash
[ssh]

enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 6
```

* `enabled`: Determines whether or not the filter is turned on.
* `port`: The port Fail2ban should be referencing in regards to the service. If using the default port, then the service name can be placed here. If using a non-traditional port, this should be the port number. For example, if you moved your SSH port to 3456, you would replace `ssh`with `3456`.
* `filter`: The name of the file located in `/etc/fail2ban/filter.d` that contains the failregex information used to parse log files appropriately. The `.conf` suffix need not be included.
* `logpath`: Gives the location of the service’s logs.
* `maxretry`: Will override the global `maxretry` for the defined service. `findtime` and `bantime` can also be added.
* `action`: This can be added as an additional setting, if the default action is not suitable for the jail. Additional actions can be found in the `action.d` folder.

{% hint style="info" %}
 Jails can also be configured as individual `.conf` files placed in the `jail.d` directory. The format will remain the same.
{% endhint %}

## Using the fail2ban Client

Fail2ban comes with a command `fail2ban-client`_`COMMAND`_ 

Options for Command's are:

* `start`: Starts the Fail2ban server and jails.
* `reload`: Reloads Fail2ban’s configuration files.
* `reload JAIL`: Replaces `JAIL` with the name of a Fail2ban jail; this will reload the jail.
* `stop`: Terminates the server.
* `status`: Will show the status of the server, and enable jails.
* `status JAIL`: Will show the status of the jail, including any currently-banned IPs.

{% hint style="info" %}
For further reading and information pertaining to commands, check out the [fail2ban wiki](http://www.fail2ban.org/wiki/index.php/Commands).
{% endhint %}



## REGEX

If you fancy a personal filter for yourself, fail2ban accepts pythons [Regular Expressions](https://www.debuggex.com/cheatsheet/regex/python) to parse log files, looking for instances of attempted break ins and password fails.

### To apply your personal REGEX filter:

`cd /etc/fail2ban/filter.d`

`sudo vim SERVICE.conf`

Where "SERVICE" is the name of the service you made your regex for.

```bash
# Fail2Ban filter for SERVICE
#
#

[Definition]

failregex = <host> - - REGEX
ignoreregex =
```

Add a SERVICE section to `jail.local`

```bash
[SERVICE]
enabled  = true
filter   = SERVICE
logpath  = /var/log/SERVICE.log
port     = 80,443
#other actions can be defined here
action   =
```

Now restart fail2ban.

## Service Starting Issues

{% hint style="warning" %}
If your `fail2ban` instance will not start, check the `jails.d` directory for a file called `00-firewalld.conf` and remove it. `iptables` is the boss of CENT, so when `fail2ban` also installs `fail2ban-firewalld`.. `firewalld` takes the cake and beats up `iptables`. 

Hit up /etc/fail2ban/jail.d and remove the violent file

`cd /etc/fail2ban/jail.d`

`sudo rm 00-firewalld.conf`
{% endhint %}

