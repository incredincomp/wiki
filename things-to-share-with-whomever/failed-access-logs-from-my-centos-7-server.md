---
description: Just any of the failed login attempts I am collecting for fail2ban
---

# Failed Access Logs From My CentOS 7 Server

## How to view these log files on your own system:

```text
cat /var/log/secure | grep 'Failed password'
cat /var/log/secure | grep 'Disconnected'
```

## Log Entries for 'Failed password':

```text
Feb 27 08:06:49 incredincomp sshd[1662]: Failed password for invalid user test from 210.22.136.74 port 50222 ssh2
Feb 27 08:07:03 incredincomp sshd[1669]: Failed password for root from 218.92.1.142 port 64516 ssh2
Feb 27 08:07:05 incredincomp sshd[1669]: Failed password for root from 218.92.1.142 port 64516 ssh2
Feb 27 08:07:07 incredincomp sshd[1669]: Failed password for root from 218.92.1.142 port 64516 ssh2
Feb 27 08:07:41 incredincomp sshd[1680]: Failed password for root from 218.92.1.142 port 56931 ssh2
Feb 27 08:07:43 incredincomp sshd[1680]: Failed password for root from 218.92.1.142 port 56931 ssh2
Feb 27 08:07:46 incredincomp sshd[1680]: Failed password for root from 218.92.1.142 port 56931 ssh2
Feb 27 08:08:23 incredincomp sshd[1688]: Failed password for root from 218.92.1.142 port 46950 ssh2
Feb 27 08:08:25 incredincomp sshd[1688]: Failed password for root from 218.92.1.142 port 46950 ssh2
Feb 27 08:08:27 incredincomp sshd[1688]: Failed password for root from 218.92.1.142 port 46950 ssh2
Feb 27 08:09:04 incredincomp sshd[1694]: Failed password for root from 218.92.1.142 port 39147 ssh2
Feb 27 08:09:06 incredincomp sshd[1694]: Failed password for root from 218.92.1.142 port 39147 ssh2
Feb 27 08:09:09 incredincomp sshd[1694]: Failed password for root from 218.92.1.142 port 39147 ssh2
Feb 27 08:09:44 incredincomp sshd[1699]: Failed password for root from 218.92.1.142 port 38730 ssh2
Feb 27 08:09:47 incredincomp sshd[1699]: Failed password for root from 218.92.1.142 port 38730 ssh2
Feb 27 08:09:49 incredincomp sshd[1699]: Failed password for root from 218.92.1.142 port 38730 ssh2
Feb 27 08:10:26 incredincomp sshd[1704]: Failed password for root from 218.92.1.142 port 30725 ssh2
Feb 27 08:10:29 incredincomp sshd[1704]: Failed password for root from 218.92.1.142 port 30725 ssh2
Feb 27 08:10:32 incredincomp sshd[1704]: Failed password for root from 218.92.1.142 port 30725 ssh2
Feb 27 08:11:06 incredincomp sshd[1711]: Failed password for root from 218.92.1.142 port 64791 ssh2
Feb 27 08:11:08 incredincomp sshd[1711]: Failed password for root from 218.92.1.142 port 64791 ssh2
Feb 27 08:11:11 incredincomp sshd[1711]: Failed password for root from 218.92.1.142 port 64791 ssh2
Feb 27 08:11:46 incredincomp sshd[1717]: Failed password for root from 218.92.1.142 port 54966 ssh2
Feb 27 08:11:48 incredincomp sshd[1717]: Failed password for root from 218.92.1.142 port 54966 ssh2
Feb 27 08:11:50 incredincomp sshd[1717]: Failed password for root from 218.92.1.142 port 54966 ssh2
Feb 27 08:12:25 incredincomp sshd[1729]: Failed password for root from 218.92.1.142 port 43857 ssh2
Feb 27 08:12:28 incredincomp sshd[1729]: Failed password for root from 218.92.1.142 port 43857 ssh2
Feb 27 08:12:30 incredincomp sshd[1729]: Failed password for root from 218.92.1.142 port 43857 ssh2
Feb 27 08:13:06 incredincomp sshd[1751]: Failed password for root from 218.92.1.142 port 41348 ssh2
Feb 27 08:13:08 incredincomp sshd[1751]: Failed password for root from 218.92.1.142 port 41348 ssh2
Feb 27 08:13:11 incredincomp sshd[1751]: Failed password for root from 218.92.1.142 port 41348 ssh2
```

## Log Entries for 'Disconnected'

```text
Feb 27 08:06:50 incredincomp sshd[1662]: Disconnected from 210.22.136.74 port 50222 [preauth]
Feb 27 08:07:08 incredincomp sshd[1669]: Disconnected from 218.92.1.142 port 64516 [preauth]
Feb 27 08:07:19 incredincomp sshd[1677]: Disconnected from 223.111.139.247 port 36298 [preauth]
Feb 27 08:07:47 incredincomp sshd[1680]: Disconnected from 218.92.1.142 port 56931 [preauth]
Feb 27 08:08:14 incredincomp sshd[1686]: Disconnected from 36.156.24.96 port 35830 [preauth]
Feb 27 08:08:27 incredincomp sshd[1688]: Disconnected from 218.92.1.142 port 46950 [preauth]
Feb 27 08:09:09 incredincomp sshd[1694]: Disconnected from 218.92.1.142 port 39147 [preauth]
Feb 27 08:09:50 incredincomp sshd[1699]: Disconnected from 218.92.1.142 port 38730 [preauth]
Feb 27 08:10:27 incredincomp sshd[1707]: Disconnected from 115.238.245.4 port 36374 [preauth]
Feb 27 08:10:32 incredincomp sshd[1704]: Disconnected from 218.92.1.142 port 30725 [preauth]
Feb 27 08:11:11 incredincomp sshd[1711]: Disconnected from 218.92.1.142 port 64791 [preauth]
Feb 27 08:11:50 incredincomp sshd[1717]: Disconnected from 218.92.1.142 port 54966 [preauth]
Feb 27 08:12:31 incredincomp sshd[1729]: Disconnected from 218.92.1.142 port 43857 [preauth]
Feb 27 08:13:11 incredincomp sshd[1751]: Disconnected from 218.92.1.142 port 41348 [preauth]
```

