---
description: >-
  Took me way too long to find this info last night so I''ll put it here for
  easier reference for myself and you if you need it!
---

# EPEL and fail2ban Install for PI 3

{% hint style="info" %}
Make sure your build is as updated as a CentOS distro can be, for whatever that is worth!
{% endhint %}

```bash
sudo yum update
```

## Add the ARM7/EPEL repo to your /etc/yum.repos.d directory

```bash
vim /etc/yum.repos.d/epel.repo
```

Add the following information, taken from [CentOS.org](https://wiki.centos.org/SpecialInterestGroup/AltArch/armhfp?action=show&redirect=SpecialInterestGroup%2FAltArch%2FArm32#head-f2a772703b3caa90cc284e01bc87423ce9a87bcd)

```bash
[epel]
name=Epel rebuild for armhfp
baseurl=https://armv7.dev.centos.org/repodir/epel-pass-1/
enabled=1
gpgcheck=0
```

## Now install Fail2Ban

```bash
yum install fail2ban
```

{% hint style="info" %}
I haven't done any configuration yet but as I accomplish more, I will keep updating things.
{% endhint %}

## Start fail2ban

```bash
systemctl enable fail2ban
systemctl start fail2ban
```

